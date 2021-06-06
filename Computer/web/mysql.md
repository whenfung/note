---
title: mysql 快速上手
---

`mysql` 是最流行的数据库之一，最近想重温一下。做一些笔记和思考。

1. 下载 `mysql`。 

   ```bash
   sudo apt-get install tasksel
   sudo tasksel
   ```

   利用 `tasksel` 下载集成 lamp，其中包括 `mysql` 。

   `mysql` 的开启和关闭利用 `sudo service mysql start/stop` 。

2. 修改编码

   以防中文出现乱码，最好一开始设置编码为 utf8。进入 `mysql` ，利用以下指令查看，一般数据库和服务器都是拉丁编码。

   ```mysql
   sudo mysql
   show variables like "%char%"
   ```

   `sudo vim /etc/mysql/my.cnf` 在最后添加以下信息，重启 `mysql` 。

   ```mysql
   [mysqld]
   character-set-server = utf8
   collation-server = utf8_general_ci
   ```

3. 创建用户和数据库。

   `sudo mysql` 进入 `mysql`。

   ```mysql
   CREATE USER 'neuron'@'localhost' IDENTIFIED BY '123456';
   CREATE DATABASE neuron;
   grant all privileges on neuron.* to 'neuron'@'localhost' identified by '123456';
   flush privileges;
   ```

   创建一个叫 neuron 的用户，拥有数据库 neuron 的所有权限。利用 exit 退出数据库。

4. 连接数据库用户

   ```bash
   mysql -u neuron -p
   ```

   -u 表示用户，-p 表示用户密码。

5. 利用 `php` 脚本连接并创建数据库。

   ```php
   <?php 
   $dbhost = 'localhost';       // mysql服务器主机地址
   $dbuser = 'neuron';            // mysql用户名
   $dbpass = '123456';          // mysql用户名密码
   $conn = mysqli_connect($dbhost, $dbuser, $dbpass);
   if(! $conn )
   {
       die('Could not connect: ' . mysqli_error());
   }
   echo '数据库连接成功！<br />';
   $sql = 'CREATE DATABASE neuron';
   $retval = mysqli_query($conn,$sql );
   if(! $retval )
   {
   	die('创建数据库失败: ' . mysqli_error($conn));
   }
   echo "数据库 neuron 创建成功\n";
   mysqli_close($conn);
   ?>
   ```

   如果用户权限不够就会失败，我这里的用户是普通用户，当然会失败，因为没有 create 权限。比较安全。

6. 删除数据库

   ```mysql
   drop database <数据库名>
   ```

   首先用户得有 drop 权限，一般这个权限只给 root，删了就没了数据，多危险。

   使用 `php` 删除数据库语句如下

   ```php
   $sql = 'DROP DATABASE neuron';
   $retval = mysqli_query( $conn, $sql );
   if(! $retval )
   {
       die('删除数据库失败: ' . mysqli_error($conn));
   }
   echo "数据库 neuron 删除成功\n";
   ```

7. 选择数据库

   一个用户可能有多个数据库，`php` 的选择数据库语句如下。

   ```php
   mysqli_select_db($conn, 'neuron' );
   ```

8. 数据类型

   了解数据类型对数据库优化很重要，`mysql` 支持很多类型，主要分为三类

   - 数值，分为整数和小数。
   - 日期/时间
   - 字符串（字符），常用的是 CHAR 和 VERCHAR，它们保存和检索的方式不同。它们的最大长度和尾部空格是否被保留等方面也不同。在存储或检索过程中不进行大小写转换。

9. 创建数据表

   数据表包括：表名、表字段名，定义每个表字段。通用语法如下

   ```mysql
   CREATE TABLE table_name (column_name column_type);
   ```

   实例：ENGINE 是引擎设置，`InnoDB` 是 `mysql` 的默认引擎。主键是 id。

   ```mysql
   CREATE TABLE IF NOT EXISTS `blog_table`(
      `blog_id` INT UNSIGNED AUTO_INCREMENT,
      `blog_title` VARCHAR(100) NOT NULL,
      `blog_author` VARCHAR(40) NOT NULL,
      `submission_date` DATE,
      PRIMARY KEY ( `blog_id` )
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;
   ```

   利用 `php` 创建数据库如下。

   ```php
   $sql = "CREATE TABLE blog_table( ".
           "blog_id INT NOT NULL AUTO_INCREMENT, ".
           "blog_title VARCHAR(100) NOT NULL, ".
           "blog_author VARCHAR(40) NOT NULL, ".
           "submission_date DATE, ".
           "PRIMARY KEY ( blog_id ))ENGINE=InnoDB DEFAULT CHARSET=utf8; ";
   mysqli_select_db( $conn, 'blog' );
   $retval = mysqli_query( $conn, $sql );
   if(! $retval )
   {
       die('数据表创建失败: ' . mysqli_error($conn));
   }
   echo "数据表创建成功\n";
   ```

   `mysql` 中利用 `show tables` 查看表，若想看表的详细信息。执行

   ```mysql
   desc <表名>;
   ```

10. 删除数据表

       ```mysql
    DROP TABLE table_name;
       ```

    `php` 实现如下

       ```php
    $sql = "DROP TABLE blog_table";
    mysqli_select_db( $conn, 'blog' );
    $retval = mysqli_query( $conn, $sql );
    if(! $retval )
    {
      die('数据表删除失败: ' . mysqli_error($conn));
    }
    echo "数据表删除成功\n";
       ```

    利用 `show tables` 查看是否删除成功。

11. 插入数据

    一般格式为：

    ```mysql
    INSERT INTO table_name ( field1, field2,...fieldN )
                           VALUES
                           ( value1, value2,...valueN );
    ```

    例子如下：

    ```mysql
    INSERT INTO blog_table (blog_title, blog_author, submission_date) VALUES ("学习 PHP", "菜鸟教程", NOW());
    ```

    查看表中数据：

    ```mysql
    select * from blog_table;
    ```

    利用 `php` 语句插入数据：

    ```php
    // 设置编码，防止中文乱码
    mysqli_query($conn , "set names utf8");
    
    $blog_title = '学习 Python';
    $blog_author = 'professordeng';
    $submission_date = '2016-03-06';
    
    $sql =  "INSERT INTO blog_table ".
            "(blog_title, blog_author, submission_date) ".
            "VALUES ".
            "('$blog_title','$blog_author','$submission_date')";
    
    mysqli_select_db($conn, 'blog');
    
    $retval = mysqli_query($conn, $sql);
    if(! $retval ) {
        die('无法插入数据: ' . mysqli_error($conn));
    }
    echo "数据插入成功\n";
    ```
    
12. 查询数据

    ```mysql
    SELECT column_name,column_name
    FROM table_name
    [WHERE Clause]
    [LIMIT N][OFFSET M]
    ```

    `php` 实现，利用 `mysqli_fetch_array()` 读取结果，每次读取一行，直到读取完毕。

    ```php
    // 设置编码，防止中文乱码
    mysqli_query($conn , "set names utf8");
    $sql =  'SELECT blog_id, blog_title, 
            blog_author, submission_date
            FROM blog_table';
    
    mysqli_select_db( $conn, 'blog' );
    $retval = mysqli_query( $conn, $sql );
    if(! $retval )
    {
        die('无法读取数据: ' . mysqli_error($conn));
    }
    echo '<h2>菜鸟教程 mysqli_fetch_array 测试<h2>';
    echo '<table border="1"><tr><td>教程 ID</td><td>标题</td><td>作者</td><td>提交日期</td></tr>';
    while($row = mysqli_fetch_array($retval, MYSQLI_ASSOC))
    {
        echo "<tr><td> {$row['blog_id']}</td> ".
        "<td>{$row['blog_title']} </td> ".
        "<td>{$row['blog_author']} </td> ".
        "<td>{$row['submission_date']} </td> ".
        "</tr>";
    }
    echo '</table>';
    ```
    
    如果你需要在字符串中使用变量，请将变量置于花括号。
    
    `PHP mysqli_fetch_array()` 函数第二个参数为 `MYSQLI_ASSOC` ， 设置该参数查询结果返回关联数组，你可以使用字段名称来作为数组的索引。如果第二个参数是 `MYSQLI_NUM` ，则以数字索引访问。
    
    以关联数组访问的函数还有 `mysqli_fetch_assoc()` 。
    
    在我们执行完 `SELECT` 语句后，释放游标内存是一个很好的习惯。
    
    可以通过 `PHP` 函数 `mysqli_free_result()` 来实现内存的释放。

13. where 子句

    查询用，符合条件就有返回，否则返回空。

    ```mysql
    SELECT * from blog_table WHERE blog_author = 'professordeng';
    ```

    
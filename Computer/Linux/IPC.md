---
title: ipc 进程通信机制
---

进程间通信（Inter-Process Communication），提供了各种进程间通信的方法。在 linux C 编程中有几种方法

1. 半双工 Unix 管道 （匿名管道）
2. FIFO （命名管道）
3. 消息队列
4. 信号量
5. 共享内存
6. 网络 Scoket

## 匿名管道

系统内核的一块小的缓冲区。用来给父子进程通信，只能一边写数据，一边读数据。写方需要把读的一端关闭，读方需要把写的一端关闭。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>

int main(){
	pid_t pid = 0;
	int fds[2];  // 0 读 1 写
	char buf[128];
	int nwr = 0;

	pipe(fds);

	pid = fork();

	if(pid < 0) {
		printf("Fork error!\n");
		return -1;
	} else if( pid == 0){
		close(fds[1]);                        // 子进程关闭写端
		nwr = read(fds[0], buf, sizeof(buf)); // 从读端读入数据
		printf("hello %s \n", buf);
	} else {
		close(fds[0]);                         // 父程序关闭读端
		strcpy(buf, "world!");
		nwr = write(fds[1], buf, sizeof(buf)); // 往写端写入数据
	}
	return 0;
}

```

## FIFO

这个类似共享文件，不同进程可以使用相同的有名管道，这也是内核分配的空间。

```c
#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
#include <fcntl.h>


int main(){
	pid_t pid = 0;
	char buf[128];
	int nwr = 0;
	int fd;
	const char* fifo = "fifoFile";
	
	mkfifo(fifo, 0666);

	pid = fork();

	if(pid < 0) {
		printf("Fork error!\n");
		return -1;
	} else if( pid == 0){
		fd = open(fifo, O_RDONLY);
		nwr = read(fd, buf, sizeof(buf)); // 从读端读入数据
		printf("hello %s \n", buf);
	} else {
		fd = open(fifo, O_WRONLY);
		strcpy(buf, "world!");
		nwr = write(fd, buf, sizeof(buf)); // 往写端写入数据
	}
	return 0;
}

```

## 共享内存

进程间的内存是不共享的，所以不能进行数据交流，共享内存就是开辟一个内存，不同进程都可以共享这块内存，达到进程间通信的目的。

ipcs -m 查看

ipcrm -m shm_id 删除某个共享内存

shmget.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/shm.h>

#define BUFSZ 4096

int main(){
	int shm_id;
	shm_id = shmget(IPC_PRIVATE, BUFSZ, 0666);  //创建共享内存
	if ( shm_id < 0) {
		perror( "shmget fail\n");
		exit ( 1 );
	}

	printf("Successfully created segment: %d \n", shm_id);

	system("ipcs -m"); // 显示系统的共享内存信息
	return 0;
}

```

shmgetWrite.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <string.h>

int main(int argc, char *argv[]){
	int shm_id;
	char* shm_buf;
	if ( argc != 2) {
		printf ("USAGE: atshm <identifier>\n");
		exit ( 1 );
	}

	shm_id = atoi(argv[1]);
	
	if ( (shm_buf = (char *)shmat( shm_id, 0, 0)) < (char* )0) { // 映射共享内存到进程空间
		perror ("shmat fail!\n");
		exit(1);
	}

	printf( "segment attached at %p\n", shm_buf);
	system("ipcs -m");
	strcpy(shm_buf, "Hello shared memory!\n");
	getchar();
	
	if ( (shmdt(shm_buf)) <0){
		perror ("shmdt");
		exit(1);
	}
	printf("segment detached \n");
	system("ipcs -m");

	getchar();
	exit(0);
}


```

shmgetRead.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <string.h>

int main(int argc, char *argv[]){
	int shm_id;
	char* shm_buf;
	
	if ( argc != 2) {
		printf ("USAGE: atshm <identifier>\n");
		exit ( 1 );
	}

	shm_id = atoi(argv[1]);
	
	if ( (shm_buf = (char *)shmat( shm_id, 0, 0)) < (char* )0) { // 映射共享内存到进程空间
		perror ("shmat fail!\n");
		exit(1);
	}

	printf( "segment attached at %p\n", shm_buf);
	system("ipcs -m");
	printf("The string in SHM is :%s \n", shm_buf);
	getchar();
	
	if ( (shmdt(shm_buf)) <0){
		perror ("shmdt");
		exit(1);
	}
	printf("segment detached \n");
	system("ipcs -m");

	getchar();
	exit(0);
}

```

## 消息队列

```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

#include <string.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/msg.h>

#define MAX_SEND_SZIE 80

struct mymsgbuf {				// 消息的结构体 
	long mtype;	             	// 消息类型 
	char mtext[MAX_SEND_SZIE];  // 消息内容
};

void send_message(int qid, struct mymsgbuf *qbuf, long type, char *text);
void read_message(int qid, struct mymsgbuf *qbuf, long type);
void remove_queue(int qid);
void change_queue_mode(int qid, char *mode);
void usage(void);

int main(int argc, char *argv[]) {
	key_t key;
	int msgqueue_id;
	struct mymsgbuf qbuf;

	if(argc == 1)
		usage();

	// Create unique key via call to ftok()
	key = ftok(".",'m');

	// open the queue - create if necessary
	if((msgqueue_id = msgget(key, IPC_CREAT|0660)) == -1) {
		perror("msgget");
		exit(1);
	}

	switch(tolower(argv[1][0])) {
		case 's': send_message(msgqueue_id, (struct mymsgbuf*)&qbuf, atol(argv[2]), argv[3]);
				  break;
		case 'r': read_message(msgqueue_id, &qbuf, atol(argv[2]));
				  break;
		case 'd': remove_queue(msgqueue_id);
				  break;
		case 'm': change_queue_mode(msgqueue_id, argv[2]);
				  break;
		default: usage();
	}
	return 0;
}

void send_message(int qid, struct mymsgbuf *qbuf, long type, char *text) {
	// send a message to the queue
	printf("Sending a message \n");
	qbuf->mtype = type;			// 填写消息的类型
	strcpy(qbuf->mtext, text);  // 填写消息内容

	if((msgsnd(qid, (struct msgbuf *)qbuf, strlen(qbuf->mtext)+1, 0)) == -1) {
		perror("msgsnd");
		exit(1);
	}
	return;
}

void read_message(int qid, struct mymsgbuf *qbuf, long type) {
	// Read a message from the queue
	printf("Reading a message \n");
	qbuf->mtype = type;
	msgrcv(qid, (struct msgbuf*)qbuf, MAX_SEND_SZIE,type, 0);

	printf("Type: %ld, Text: %s\n", qbuf->mtype, qbuf->mtext);
	return;
}

void remove_queue(int qid) {
	// remove the queue
	msgctl(qid, IPC_RMID, 0);
	return;
}

void change_queue_mode(int qid, char *mode) {
	struct msqid_ds myqueue_ds;

	// get current info
	msgctl(qid, IPC_STAT, &myqueue_ds);

	// Convert and load the mode 
	sscanf(mode, "%ho", &myqueue_ds.msg_perm.mode);

	// update the mode 
	msgctl(qid, IPC_SET, &myqueue_ds);
	return ;
}

void usage(void) {
	fprintf(stderr, "msgtool - A utility for tinkering with msg queues\n");
	fprintf(stderr, "nUSAGE: msgtool (s)end \n");
	fprintf(stderr, "        msgtool (r)ecv \n");
	fprintf(stderr, "		 msgtool (d)elete \n");
	fprintf(stderr, "		 msgtool (m)ode \n");
	exit(1);
}
```

## 例题

1. 进程 A 创建子进程 B, 子进程 B 创建孙进程 C ，AB 间用管道 1 通信，BC 间用管道2 通信，实现进程 A 将一个消息字符串发送到 C 进程，并有 C 进程打印出来以验证通信成功。要求：管道 2 只对 BC 可见，即对 A 是不可见的。提示：子进程继承（复制）父进程资源。
2. A进程创建一个消息队列（邮箱）；B进程向邮箱按顺序发送三条类型分别为111/222/333 的消息，内容不作限定；C进程按333/111/222的顺序读取消息；D进程删除该消息队列（邮箱）。
3. 借助google工具查找资料，学习线程间通信和同步方法。
4. 设计编写以下程序，着重考虑其同步问题
   1. 一个程序（进程）从客户端读入按键信息，一次将 “一整行” 按键信息保存到一个共享存储的缓冲区内并等待读取进程将数据读走，不断重复上面的操作。
   2. 另一个程序（进程）生成两个进程/线程，用于显示缓冲区内的信息，这两个线程并发读取缓冲区信息后将缓冲区清空（一个线程的两次显示操作之间可以加入适当的时延以便于观察）。
   3. 在两个独立的终端窗口上分别运行上述两个程序，展示其同步与通信功能，要求一次只有一个任务在操作缓冲区。
   4. 运行程序，记录操作过程并给出文字说明。


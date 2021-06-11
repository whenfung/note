---
title: Audio
---

HTML 支持音频，例如 QQ 空间、博客的背景音乐。实现音频插入的方法很多，这里我们讲解如何插入 mp3 音频。

## audio

HTML5 支持 audio 标签，格式如下：

```html
<audio controls autoplay loop> 
    <source src="https://music.163.com/song/media/outer/url?id=187672.mp3"> 
</audio>
```

这里有几个控制元素，分别如下：

| 属性     | 值       | 描述                   |
| -------- | -------- | ---------------------- |
| autoplay | autoplay | 音频就绪后自动播放     |
| controls | controls | 显示控件，例如播放按钮 |
| loop     | loop     | 循环播放               |
| muted    | muted    | 规定视频输出应该被静音 |

利用网易云音乐提供的音乐直链，实现样例如下：

<audio controls autoplay loop> 
    <source src="https://music.163.com/song/media/outer/url?id=187672.mp3"> 
</audio>


# jQuery-music

注意：豆瓣会限制我们的访问，所以在<head>标签下一定要添加<meta name="referrer" content="no-referrer">

html5中audio标签本身提供进度条功能，以及音量控制功能的，这里我为了界面的好看自己设置了进度条，音量控制还没有加，大家可以自行添加。歌词信息获取方式：
http://jirenguapi.applinzi.com/fm/getLyric.php?ssid=4f86&sid=1451876
这里的sid&ssid获取歌曲信息可以得到。
然后我们需要在js文件结尾加上$(document).ready(getChannel())代码让浏览器预加载播放器。这里基本已经把播放器完成了，功能比较简单。有兴趣的同学可以自己再添加功能。

作者：张新望zxw
链接：https://www.jianshu.com/p/29cd724580fc

PS：这个播放器最厉害的一点就是你永远猜不到下一首歌是什么，是不是很吊？？？哈哈哈

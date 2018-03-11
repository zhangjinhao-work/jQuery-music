# jQuery-music
html部分
代码：
<div class="wrapper">

<div class="background"></div>
<div class="content">
<audio src=""></audio>
<div class="music-massage">
<p class="musicname"></p>
<p class="musicer"></p>
</div>

<div class="music-icon">
<a class="m-icon m-fenxiang colored" href="http://service.weibo.com/share/share.php?title=#_loginLayer_1466697157538" target="new"></a>
<span class="m-icon m-star colored"></span>
<span class="m-icon m-heart colored"></span>
</div>
</div>

<span class="basebar">
<span class="progressbar"></span>
</span>
<div class="controls">

<div class="play-control">
<span class="m-icon m-play btn1" title="播放/暂停"></span>
<span class="m-icon m-change btn2" title="换频道"></span>
<span class="m-icon m-next btn3" title="换曲"></span>
</div>

<div class="music-control">
<span class="m-icon m-xunhuan colored"></span>
<span class="m-icon m-radom colored"></span>
</div>
</div>
</div>
这里就不写css的代码了，大家可以直接看源文件或者从开发者工具中去看。如果有问题可以私聊我。
js部分
代码一（播放控制）：
//播放控制
var myAudio = $("audio")[0];
// 播放/暂停控制
$(".btn1").click(function(){
if (myAudio.paused) {
play()
} else {
pause()
}
});
// 频道切换
$(".btn2").click(function(){
getChannel();
});
// 播放下一曲音乐
$(".btn3").click(function(){
getmusic();
});
function play(){
myAudio.play();
$('.btn1').removeClass('m-play').addClass('m-pause');
}
function pause(){
myAudio.pause();
$('.btn1').removeClass('m-pause').addClass('m-play');
}
代码二（ajax获取豆瓣fm音乐）：
//获取随机频道信息
function getChannel(){
$.ajax({
url: 'http://api.jirengu.com/fm/getChannels.php',
dataType: 'json',
Method: 'get',
success: function(response){
var channels = response.channels;
var num = Math.floor(Math.random()*channels.length);
var channelname = channels[num].name;//获取随机频道的名称
var channelId = channels[num].channel_id;//获取随机频道ID
$('.record').text(channelname);
$('.record').attr('title',channelname);
$('.record').attr('data-id',channelId);//将频道ID计入data-id中
getmusic();
}
})
}
// 通过ajax获取歌曲
function getmusic(){
$.ajax({
url: 'http://api.jirengu.com/fm/getSong.php',
dataType: 'json',
Method: 'get',
data:{
'channel': $('.record').attr('data-id')
},
success: function (ret) {
var resource = ret.song[0],
url = resource.url,
bgPic = resource.picture,
sid = resource.sid,//获取歌词的参数
ssid = resource.ssid,//获取歌词的参数
title = resource.title,
author = resource.artist;
$('audio').attr('src',url);
$('.musicname').text(title);
$('.musicname').attr('title',title)
$('.musicer').text(author);
$('.musicer').attr('title',author)
$(".background").css({
'background':'url('+bgPic+')',
'background-repeat': 'no-repeat',
'background-position': 'center',
'background-size': 'cover',
});
play();//播放
}
})
};
注意：豆瓣会限制我们的访问，所以在<head>标签下一定要添加<meta name="referrer" content="no-referrer">
代码三（进度条控制）：
setInterval(present,500) //每0.5秒计算进度条长度
$(".basebar").mousedown(function(ev){ //拖拽进度条控制进度
var posX = ev.clientX;
var targetLeft = $(this).offset().left;
var percentage = (posX - targetLeft)/400100;
myAudio.currentTime = myAudio.duration * percentage/100;
});
function present(){
var length = myAudio.currentTime/myAudio.duration100;
$('.progressbar').width(length+'%');//设置进度条长度
//自动下一曲
if(myAudio.currentTime == myAudio.duration){
getmusic()
}
}
html5中audio标签本身提供进度条功能，以及音量控制功能的，这里我为了界面的好看自己设置了进度条，音量控制还没有加，大家可以自行添加。歌词信息获取方式：
http://jirenguapi.applinzi.com/fm/getLyric.php?ssid=4f86&sid=1451876
这里的sid&ssid获取歌曲信息可以得到。
然后我们需要在js文件结尾加上$(document).ready(getChannel())代码让浏览器预加载播放器。这里基本已经把播放器完成了，功能比较简单。有兴趣的同学可以自己再添加功能。

作者：张新望zxw
链接：https://www.jianshu.com/p/29cd724580fc
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
PS：这个播放器最厉害的一点就是你永远猜不到下一首歌是什么，是不是很吊？？？哈哈哈

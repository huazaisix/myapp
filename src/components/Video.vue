<template>
  <div class="vc">
    <div class="one">
      <div class="video_tab">
        <video crossorigin="*"  id="video_src" controls width="100%" height="600px" src="../../static/video/demo.mp4" v-show="false">

        </video>
        <canvas id="video_canvas"></canvas>
        <div class="video_loading" v-if="loadingCss">
          <svg class="svg_loading">
            <linearGradient id="wow" gradientTranform="rotate(45)">
              <stop stop-color="white"></stop>
              <stop stop-color="dimgray" offset="2"></stop>
            </linearGradient>
            <circle id="loading_circle" cx="50" cy="30" r="25" fill="None"/>
          </svg>
          <span>视频缓冲中...</span>
        </div>
      </div>

      <div class="info_tab">
        aa
      </div>

      <div class="nav_tab">
        <div class="nav_name" v-for="(value, index) in nav_list" @click="add_btn(index)">
          {{value.name}}{{index}}
        </div>
      </div>
      <!-- <button @click="cover">截图</button> -->
    </div>
    <div class="two">
      <!-- 尺子区域 -->
      <div class="opreate_tab">
        <div class="show_time">
          {{ video_current_time | demo }}
        </div>
        <div class="scale">
          <div class="scale_content">
            <ul class="secondUl">
              <li v-for="index in video_time_already" >
                <span :class="scale_css(index)"></span>
                <span class="span_ten_num">{{index | filter_ten_num}}</span>
              </li>
            </ul>
          </div>

        </div>
      </div>

      <!-- 轨道区域 -->
      <div class="tracks">

        <div v-for="(value, index) in video_tracks_list" class="video_tracks">
          <div class="video_one">{{value}}</div>
          <div class="video_one_content">

          </div>
        </div>
        <div v-for="(value, index) in subtitle_tracks_list" class="subtitle_tracks">
          <div class="sbt_one">{{value}}</div>
          <div class="sbt_one_content">

          </div>
        </div>
        <div v-for="(value, index) in logo_tracks_list" class="logo_tracks">
          <div class="logo_one">{{value}}</div>
          <div class="logo_one_content">
            <div>
              1111111111111111
            </div>
          </div>
        </div>
        <svg class="line_svg">
          <line x1="0" y1="0" x2="0" y2="302" style="stroke:red;stroke-width: 12"/>
        </svg>
      </div>

    </div>

  </div>
</template>

<script>
import Hls from "hls.js"
import foobar from "./Nav"
import $ from "jquery"
// import { mapState } from "vuex"


export default{
  name: "vc",
  components:{
    foobar
  },
  data(){
    return {
      title: "demo",
      video_obj: "",
      interval_canvas: "",
      nav_list: [
        {
             "name":"入点",
             "pic": "static/picture/nav/in.svg",
             "click": "",
         },
         {
             "name":"出点",
             "pic": "static/picture/nav/out.svg",
             "click": "",
         },
         {
             "name":"分割",
             "pic": "static/picture/nav/split.svg",
             "click": "",
         },
         {
             "name":"起-入",
             "pic": "static/picture/nav/in_and_out.svg",
             "click": "",
         },
         {
             "name":"入-出",
             "pic": "static/picture/nav/in_and_out.svg",
             "click": "",
         },
         {
             "name":"出-尾",
             "pic": "static/picture/nav/in_and_out.svg",
             "click": "",
         },
         {
             "name":"帧退",
             "pic": "static/picture/nav/rate_left.svg",
             "click": "",
         },
         {
             "name":"后退",
             "pic": "static/picture/nav/left.svg",
             "click": "",
         },
         {
             "name":"播放",
             "pic": "static/picture/nav/stop.svg",
             "click": "",
         },
         {
             "name":"快进",
             "pic": "static/picture/nav/right.svg",
             "click": "",
         },
         {
             "name":"帧进",
             "pic": "static/picture/nav/rate_right.svg",
             "click": "",
         },
         {
             "name":"音量",
             "pic": "static/picture/nav/void.svg",
             "click": "",
         },
         {
             "name":"撤销",
             "pic": "static/picture/nav/cancel.svg",
             "click": "",
         },
         {
             "name":"重做",
             "pic": "static/picture/nav/redo.svg",
             "click": "",
         }
      ],
      // 视频的参数
      video_time: 0,
      video_time_int: 0,
      scaleSize: "20px",
      video_time_already: 100,
      video_is_play: false,
      video_is_ended: false,
      video_current_time: 0,
      // 定时器--监听视频的就绪状态
      interval_videoState: "",
      videoState: 0,
      loadingCss: false,
      // 轨道的变量
      video_tracks_list: ["视频轨"],
      subtitle_tracks_list: ["字幕轨"],
      logo_tracks_list: ["图片轨"],

    }
  },
  created: function(){

  },
  watch: {
    video_is_play: function(nv, ov){
      // 如果视频正在播放，则更改内容
      if(this.video_is_play){
        this.nav_list[8].name = "暂停";
        this.interval_canvas = setInterval(this.cover_video, 20);
      }else{
        this.nav_list[8].name = "播放";
        clearInterval(this.interval_canvas);
      }
    },
    video_is_ended: function(nv, ov){
      if(this.video_is_ended){
        this.nav_list[8].name = "播放";
        this.video_is_play = false;
        this.loadingCss = false;
        clearInterval(this.interval_canvas);
      }
    },
    videoState: function(nv){
      if(nv==4){
        this.cover_video();
        this.loadingCss = false;
      }else{
        this.loadingCss = true;
      }
    },
  },
  mounted: function(){
    this.video_obj = document.getElementById("video_src");

    this.interval_videoState = setInterval(()=>{
      // 监听视频的状态
      this.videoState = this.video_obj.readyState;
      this.video_time = this.video_obj.duration;
      // 卡尺刻度的显示
      $(".secondUl li").css("marginLeft", this.scaleSize);
    }, 20);
    // this.paly_m3u8();
    this.cover_video();
    console.log(this.video_time);

    // $(".line_svg").css("left", "20%");

    if(!this.video_obj.paused){
      this.video_is_play = true
    }


  },
  methods:{
    paly_m3u8: function(){
      if(Hls.isSupported){
        var hls = new Hls();
        hls.attachMedia(this.video_obj);
        hls.on(Hls.Events.MEDIA_ATTACHED, function() {
          console.log("开始...");
          hls.loadSource("https://yunqivedio.alicdn.com/2017yq/v2/0x0/96d79d3f5400514a6883869399708e11/96d79d3f5400514a6883869399708e11.m3u8");
          // $("video").play();
        });
      }
    },
    cover_video: function(){
      this.video_time_already = parseInt(this.video_time.toString().split(".")[0]) * 10;
      this.video_time_int = Math.ceil(this.video_time);

      var canvas_obj = document.getElementById("video_canvas");

      canvas_obj.width = 900;
      canvas_obj.height = 500;

      var ctx = canvas_obj.getContext("2d");

      // 适配其他屏幕的操作部分

      // 屏幕的设备像素比
      var deviceRatio = window.devicePixelRatio || 1;
      // 画布的像素比
      var canvasRatio = ctx.webkitBackingStorePixelRatio ||
                    ctx.mozBackingStorePixelRatio ||
                    ctx.msBackingStorePixelRatio ||
                    ctx.oBackingStorePixelRatio ||
                    ctx.backingStorePixelRatio || 1;

      var ratio = deviceRatio / canvasRatio;
      ctx.drawImage(this.video_obj,0, 0, 900,500);
      this.video_current_time = this.video_obj.currentTime;
      this.video_is_ended = this.video_obj.ended;
      console.log("视频截图中");
    },
    add_btn: function(index){

      // 帧退
      if(index==6){
        this.video_is_play = false;
        this.cover_video();
        this.video_obj.currentTime -= 1;
      }

      // 后退
      if(index==7){
        this.video_is_play = false;
        this.cover_video();
        this.video_obj.currentTime -= 0.02;
      }
      // 视频播放
      if(index==8 && this.nav_list[index].name == ("播放")){
        if(this.video_obj.paused){
          this.video_obj.play();
          this.video_is_play = true;
        }
      }else if(index==8 && this.nav_list[index].name == "暂停"){
        if(!this.video_obj.paused){
          this.video_obj.pause();
          this.video_is_play = false;
        }
      }
      // 快进
      if(index==9){
        this.video_is_play = false;
        this.cover_video();
        this.video_obj.currentTime += 0.02;
      }
      // 帧进
      if(index==10){
        this.video_is_play = false;
        this.cover_video();
        this.video_obj.currentTime += 1;
      }
    },
    scale_width: function(){
      // this.scaleSize*this.video_time*10;
    },
    scale_css: function(index){
      if(index%10 === 0){
        return "show_scale_one";
      }else if(index%5 === 0){
        return "show_scale_two";
      }else{
        return "show_scale_three";
      }
    },
    re_id: function(index){
      return "id_".concat(index.toString());
    },

  } ,
  filters: {
    demo: function(value){
      // 转化为00:00:00时间格式
       var h = parseInt((value/3600).toString().split(".")[0]);
       var m = parseInt((value/60).toString().split(".")[0]);
       var ss = (value - (h*3600) - (m*60)).toString().split(".");
       var s1 = parseInt(ss[0]);
       if(ss.length==2){
         var s2 = ss[1].substring(0,2);
       }else{
         s2 = 0;
       }
       if(0<m<10){
           var str_01 = ":0"
       }else{
           str_01 = ":"
       }

       if(0<s1<10){
         var str_02 = ":0"
       }else{
           str_02 = ":"
       }

       return h + str_01 + m + str_02 + s1 + ":" + s2;
    },
    filter_ten_num(index){
      if(index%10 === 0){
        var value = index/10;
        var h = parseInt((value/3600).toString().split(".")[0]);
        var m = parseInt((value/60).toString().split(".")[0]);
        var ss = (value - (h*3600) - (m*60)).toString().split(".");
        var s1 = parseInt(ss[0]);
        if(0<m<10){
            var str_01 = ":0"
        }else{
            str_01 = ":"
        }

        if(0<s1<10){
          var str_02 = ":0"
        }else{
            str_02 = ":"
        }
        return h + str_01 + m + str_02 + s1 ;
      }
    },
  }
}
</script>

<style scoped>
html,body,ul,li,p,div,span,video,svg,line{padding:0;margin:0;}

.one{
  display: grid;
  grid-template-columns: 60% 40%;
  grid-auto-rows: minmax(50px, auto);
  grid-gap: 0;
}
.video_tab{
  text-align: center;
  width: 100%;
  height: 500px;
  background: black;
}
#video_canvas{
  display: inline-block;
  margin: 0 auto;
  line-height: 500px;
}
.nav_tab{
  height: 50px;
  width: 100%;
  background: #eee;
  display: grid;
  grid-template-columns: repeat(16, 1fr);
  grid-gap: 2em;
  padding: 1em;
  text-align: center;
}
.nav_name{
  cursor: pointer;
}
.video_loading{
  position: relative;
  width: 100px;
  height: 100px;
  /* background-color: gray; */
  left: 45%;
  top: -55%;
}

.svg_loading{
  width: 100px;
  height: 70px;
}
@keyframes move-ants {
  to {stroke-dashoffset: -15px;}
}
#loading_circle{
  stroke: url(#wow);
  stroke-width: 4;
  stroke-dasharray: 10px 5px;
  animation: move-ants .4s infinite linear;
}
.video_loading span{
  /* background: black; */
  display: inline-block;
  /* mix-blend-mode: multiply; */
}

.video_loading span{
  float: left;
  color: white;
}
@keyframes loading {
  0% {transform: rotate(0);}
  10% {transform: rotate(0.1turn);}
  20% {transform: rotate(0.2turn);}
  30% {transform: rotate(0.3turn);}
  40% {transform: rotate(0.4turn);}
  50% {transform: rotate(0.5turn);}
  60% {transform: rotate(0.6turn);}
  70% {transform: rotate(0.7turn);}
  80% {transform: rotate(0.8turn);}
  90% {transform: rotate(0.9turn);}
  100% {transform: rotate(1turn);}
}

/* 尺子和时间的显示 */
.opreate_tab{
  height: 60px;
  width: 100%;
  color: white;
  display: grid;
  grid-template-columns: 1fr 9fr;
  grid-row-gap: 1px;
}
.show_time{
  line-height: 60px;
  padding-left: 20%;
  font-size: 20px;
  background: dimgray;
}
.scale{
  overflow: hidden;
  height: 60px;
  background: black;
}
/* .scale_content{
  background: black;
  height: 60px;
  width: auto;
} */


.tracks{
  width: 100%;
  height: auto;
}
.video_tracks,.subtitle_tracks,.logo_tracks{
  height: 100px;
  line-height: 100px;
  width: 100%;
  float: left;
  margin-bottom: 1px;
}
.video_one,.sbt_one,.logo_one{
  width: 10%;
  height: 100px;
  text-align: center;
  background: black;
  color: white;
  float: left;
}
.video_one_content,.sbt_one_content,.logo_one_content{
  width: 90%;
  height: 100px;
  float: left;
  background: gray;
}
/* 条 */
.line_svg{
  width: 4px;
  height: 302px;
  position: absolute;
  left: 11%;
}


/* 刻度的宽度 */
.show_scale_one{
  border-left:2px solid black;
  height:25px;
  width:0px;
  display: inline-block;
}
.show_scale_two{
  border-left:2px solid #2A2C2C;
  height:20px;
  width:0px;
  display: inline-block;
}
.show_scale_three{
  border-left:2px solid gray;
  height:16px;
  width:0px;
  display: inline-block;
}

.scale_content .secondUl{
  height: 40px;
  background: orange;
  position: relative;
  /* animation: test 5s infinite; */
  /* overflow: hidden; */
  top: 20px;
}
@keyframes test {
  10% {margin-left: -20px;}
  20% {margin-left: -100px;}
  40% {margin-left: -180px;}
  80% {margin-left: -300px;}
}
.secondUl li{
  /* float: left; */
  display: inline;
  white-space: nowrap;
  list-style: none;
}
.firstUl li{
  display: inline;
  white-space: nowrap;
  position: relative;
  background: gray;
}
.span_ten_num{
  /* left: -25px; */
  position: absolute;
  top: -22px;
}
</style>

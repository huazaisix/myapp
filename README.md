# chaitiao

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

# 函数方法
scale_css(int):卡尺的刻度显示样式的改变;

scale_width():未定义;

add_btn(index):nav的按钮增加事件;

cover_video():canvas画布的生成视频;

paly_m3u8():播放m3u8的视频调用函数;

re_id: 刻度上显示时间进行返回值改变;

# 基本属性
video_obj: video标签的对象;

interval_canvas: 定时器-不断截取视频图片（20ms）;

interval_videoState: 定时器-不断获取视频的就绪状态(20ms);

nav_list: nav的功能区的变量;

video_time: 视频的时长;

video_time_int: 视频时长（整数）;

scaleSize: 卡尺的基本单位px;

video_time_already: 卡尺的显示数量;

video_is_play: 视频是否正在播放;

video_is_ended: 视频是否结束;

video_current_time: 视频当前播放的时间点;

videoState: 视频的就绪状态;

loadingCss: 加载css的样式(视频未就绪则显示);

video_tracks_list: 视频轨(列表);

subtitle_tracks_list: 字幕轨(列表);

logo_tracks_list: 图片轨(列表);

# 监听的属性
video_is_play: 监听视频是否正在播放;

video_is_ended: 监听视频是否正在结束;

videoState: 监听视频的就绪状态;

# 过滤器
demo: 显示视频时间进行格式化;

filter_ten_num: 过滤可以被10整除的数字;

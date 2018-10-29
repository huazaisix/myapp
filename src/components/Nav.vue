<template>
  <!-- <div class="nav_test">
    <p>Test ....</p>
  </div> -->
  <div class="nav_css">

  </div>
</template>

<script type="text/javascript">
import video_obj from "./Video"

  export default{
    name: 'foobar',
    props: [
      {video_time_num: "video_time"}
    ],
    data(){
      return{

      }
    },
    mounted: function(){
      this.to_int();
      alert(this.video_time_num);
    },
    methods: {
      to_int: function(){
        alert(this.video_time);
        alert("....as");
        if(this.video_time){
          alert(this.video_time);
        }
      },
    },
    filters: {
      filter_number: function(value){
        var vs = value.toString().split(".");
        var number;
        if(vs.length>=2){
           number= vs[0];
        }else{
          number = value;
        }
        return number;
      }
    },
  }
</script>

<template>
<div id="app" @keyup.space="begin">
    <!-- <img src="./assets/logo.png"> -->
    <div class="tagBall" >
        <a class="tag" v-for="item in results">{{item.name}}</a>
    <!-- <a class="tag">{{results}}</a> -->
    </div>
    <div class="lucky">{{lucky}}</div>
    <!-- <transition
        name="custom-classes-transition"
        enter-active-class="animated tada"
        leave-active-class="animated bounceOutRight"
    >
        <p v-if="show">{{lucky}}</p>
    </transition> -->
        
</div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import axios from "axios";
// import ball from "./../static/js/tagCloud.js";
// import ball from "./../static/js/ball2.js";
import ball from "./../static/js/ball.js";
export default {
  name: "App",
  components: {
    HelloWorld
  },
  data() {
    return {
      results: null,
      lucky: "",
      show: true,
      timer: null
    };
  },
  mounted: function() {
    axios
      .post("https://me.rehack.cn/api/v1/users")
      .then(response => {
        this.results = response.data;
      })
      .then(function() {
        ball.ball();
      });
    window.onkeydown = e => {
    //   alert(e.keyCode);
      if (e.keyCode == 32) {
        this.begin();
      }
      if (e.keyCode == 27) {
          window.localStorage.clear()
      }
    };
  },
  methods: {
    begin: function() {
      var _this = this;
      var data = _this.results;
      var st = window.localStorage;
      clearInterval(this.timer);

      if (_this.lucky.length >= 5) {
        return false;
      }
      this.lucky = [];
      this.timer = setInterval(function() {
        // console.log(data.length)
        var a = parseInt(Math.random() * (data.length + 1));
        console.log(a);
        console.log(data[a]);
        _this.lucky.push(data[a].name);
        data.splice(a, 1);
        if (_this.lucky.length >= 5) {
          // alert(1)
          clearInterval(_this.timer);
          ball.ball();
          st.setItem("lucky", _this.lucky.join(","));
        }
      }, 1200);

      // st.setItem('lucky',this.lucky)
    },
    play: function() {}
  }
};
</script>

<style>
@import url("../static/css/lucky.css");
@import url("https://cdn.jsdelivr.net/npm/animate.css@3.5.1");
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.bounce-enter-active {
  animation: bounce-in 0.5s;
}

.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}

@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>

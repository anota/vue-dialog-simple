<template>
  <div id="vueAlert">
    <transition-group name="fade" tag="div">
      <div class="confirm" key="1" v-show="show"></div>
    </transition-group>
    <transition-group name="bounce" tag="div">
      <div class="confirm-dialog" key="2" v-show="show">
        <div class="flex-1 c-container">
          <p class="c-title"  :style="{color:titleColor}"  v-if="title">{{title}}</p>
          <div class="c-con" :style="{color:textColor}"  v-if="text">{{text}}</div>
          <div v-if="html" v-html="html"></div>
        </div>
        <div class="btn-confirm flex">
          <template>
            <span class="flex-1" :style="{color:sureColor}" @click.stop="sureFunc()">{{sureText}}</span>
          </template>
        </div>
      </div>
    </transition-group>
  </div>
</template>
<script>
export default {
  props: {
    show: {
      // 是否显示此confirm
      type: Boolean,
      default: false
    },
    title: {
      type: String,
      default: ""
    },
    text: {
      // 提醒文字
      type: String,
      default: ""
    },
    html: {
      // 提醒文字
      type: String,
      default: ""
    },
    titleColor:{
      type: String,
      default: "#333"
    },
    textColor:{
      type: String,
      default: "#999"
    },
    sureText: {
      type: String,
      default: "确定"
    },
    sureColor: {
      type: String,
      default: "#F9940E"
    },
    callback: {
      type: Function,
      default() {
        return function() {};
      }
    }
  },
  methods: {
    
    sureFunc() {
      this.callback();
      this.show = false;
    },
    clearData() {
      //清除上一次进入时所保留的值
      setTimeout(() => {
        this.title = ""
        this.text = ""
       
        this.html = ""
        this.titleColor = '#F9940E'
        this.textColor = "#999"

        this.sureText = "确定"
        this.sureColor = "#F9940E"

        this.callback = function() {}
      }, 500);
    }
  },
  watch: {
    show() {
      if (!this.show) {
        this.clearData();
      }
    }
  }
}
</script>
<style scoped>
.bounce-enter-active {
  animation: bounce-in 0.1s;
}
.bounce-leave-active {
  animation: bounce-in 0.1s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0.8);
    opacity: 0.1;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.confirm {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1008;
  height: 100%;
  width: 100%;
  background: rgba(0, 0, 0, 0.6);
}
.confirm-dialog {
  position: fixed;
  display: table;
  z-index: 5000;
  width: 70%;
  max-width: 300px;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  background-color: #fff;
  text-align: center;
  border-radius: 3px;
  overflow: hidden;
}
.c-container {
  padding: 15px 0;
}
.c-container .c-title {
  font-size: 16px;
  padding-bottom: 5px;
  margin: 5px 15px 0 15px;
}
.c-container .c-con {
  font-size: 14px;
  color: #999;
  line-height: 1.6;
  margin: 0 15px;
}
.btn-confirm {
  text-align: center;
  width: 100%;
  justify-content: space-around;
  border-top: 1px solid #eee;
}

.btn-confirm span:last-child {
  border-left: 1px solid #eee;
}
.btn-confirm span {
  height: 48px;
  line-height: 48px;
  font-size: 16px;
  display: block;
}
</style>

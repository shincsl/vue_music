<template>
  <div class="top-container">
    <div class="left-box">
      <div class="icon-wrapper">
        <span @click="home" class="iconfont icon-home"></span>
        <span @click="screen" class="iconfont icon-full-screen"></span>
      </div>
      <div class="history-wrapper">
        <span @click="back" class="iconfont icon-arrow-lift"></span>
        <span @click="next" class="iconfont icon-arrow-right"></span>
      </div>
    </div>
    <div class="right-box">
      <div class="el-input el-input--small el-input--prefix">
        <!-- 搜索框 -->
        <input
          type="text"
          autocomplete="off"
          placeholder="搜索"
          class="el-input__inner"
          v-model="inputValue"
          @keyup.enter = "toResult"
        />
        <span class="el-input__prefix">
          <i class="el-input__icon el-icon-search"></i>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'top',
    data() {
      return {
        // 输入的内容
        inputValue: ''
      }
    },
    methods:{
      toResult(){
        // 非空判断
        if(this.inputValue==''){
          // 提示用户
          this.$message.warning("请输入内容")
        }else {
          // 去搜索页 携带数据
          this.$router.push('/result?q='+this.inputValue)
        }
      },
        //上一页面
        back(){
          this.$router.go(-1)
        },
        //下一页面
        next(){
            this.$router.go(1)
        },
        //回到主页
        home(){
            this.$router.push(`/discovery`)
        },
        //全屏
        screen() {
            let element = document.documentElement;
            if (this.fullscreen) {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                } else if (document.webkitCancelFullScreen) {
                    document.webkitCancelFullScreen();
                } else if (document.mozCancelFullScreen) {
                    document.mozCancelFullScreen();
                } else if (document.msExitFullscreen) {
                    document.msExitFullscreen();
                }
            } else {
                if (element.requestFullscreen) {
                    element.requestFullscreen();
                } else if (element.webkitRequestFullScreen) {
                    element.webkitRequestFullScreen();
                } else if (element.mozRequestFullScreen) {
                    element.mozRequestFullScreen();
                } else if (element.msRequestFullscreen) {
                    element.msRequestFullscreen();
                }
            }
            this.fullscreen = !this.fullscreen;
        }

    }
  }
</script>

<style scoped></style>

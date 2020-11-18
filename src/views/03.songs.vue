<template>
  <div class="songs-container">
    <div class="tab-bar">
      <span class="item" @click="tag=0" :class="{active:tag==0}">全部</span>
      <span class="item" @click="tag=7" :class="{active:tag==7}">华语</span>
      <span class="item" @click="tag=96" :class="{active:tag==96}">欧美</span>
      <span class="item" @click="tag=8" :class="{active:tag==8}">日本</span>
      <span class="item" @click="tag=16" :class="{active:tag==16}">韩国</span>
    </div>
    <!-- 底部的table -->
    <table class="el-table playlit-table">
      <thead>
        <th></th>
        <th></th>
        <th>音乐标题</th>
        <th>歌手</th>
        <th>专辑</th>
        <th>时长</th>
      </thead>
      <tbody>
        <tr v-for="(item,index) in lists" :key="index" class="el-table__row">
          <td>{{ index+1 }}</td>
          <td>
            <div class="img-wrap">
              <el-image :src="item.album.picUrl" alt="" lazy></el-image>
              <!-- 播放按钮 -->
              <span @click="playMusic(item.id)" class="iconfont icon-play"></span>
            </div>
          </td>
          <td>
            <div class="song-wrap">
              <div class="name-wrap">
                <span>{{ item.name }}</span>
                <span class="iconfont icon-mv"></span>
              </div>
              <span>{{ item.album.artists[0].name }}</span>
            </div>
          </td>
          <td>{{ item.album.artists[0].name }}</td>
          <td>{{ item.album.name }}</td>
          <td>{{ item.duration }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import axios from 'axios'
export default {
  name: 'songs',
  data() {
    return {
      // 歌曲列表
      lists:[],
      // 分类
      tag:'0'
    };
  },
  watch:{
    tag(){
      this.getList()
    }
  },
  created() {
    this.getList()
  },
  methods:{
    // 获取列表数据
    getList(){
      axios({
        url: "https://autumnfish.cn/top/song",
        method: "get",
        params: {
          // 传递设置的数据
          type: this.tag
        }
      }).then(res => {
        //console.log(res)
        this.lists = res.data.data
        // 处理时长 毫秒 转为 分秒
        for (let i = 0; i < this.lists.length; i++) {
          let duration = this.lists[i].duration
          // 假定 100000 毫秒
          // 秒 100000/1000  100秒
          // 分 100 /60
          // 秒 100 %6
          let min = parseInt(duration / 1000 / 60)
          if (min < 10) {
            min = '0' + min
          }
          let sec = parseInt((duration / 1000) % 60)
          if (sec < 10) {
            sec = '0' + sec
          }
          this.lists[i].duration = `${min}:${sec}`
        }
      })
    },
    playMusic(id){
      axios({
          url:'https://autumnfish.cn/song/url',
          method:'get',
          params:{
            id
          }
      }).then(res=>{
        let url = res.data.data[0].url
        // 设置给父组件的 播放地址
        this.$parent.musicUrl = url
      })
    }
  }
};
</script>

<style >

</style>

<template>
  <div class="mv-container">
    <div class="mv-wrap">
      <h3 class="title">mv详情</h3>
      <!-- mv -->
      <div class="video-wrap">
        <video
          controls
          autoplay
          :src="url"
        ></video>
      </div>
      <!-- mv信息 -->
      <div class="info-wrap">
        <div class="singer-info">
          <div class="avatar-wrap">
            <!-- 头像 -->
            <img :src="icon" alt="" />
          </div>
          <!-- 歌手名-->
          <span class="name">{{mvInfo.artistName}}</span>
        </div>
        <div class="mv-info">
          <!-- 标题-->
          <h2 class="title">{{mvInfo.name}}</h2>
          <!-- 发布时间 -->
          <span class="date">发布：{{mvInfo.publishTime}}</span>
          <!-- 播放次数 -->
          <span class="number">播放：{{mvInfo.playCount}}次</span>
          <!-- 描述-->
          <p class="desc">
            {{mvInfo.desc}}
          </p>
        </div>
      </div>
      <!-- 精彩评论 -->
      <div class="comment-wrap">
        <p class="title">精彩评论<span class="number">(666)</span></p>
        <div class="comments-wrap">
          <!-- 评论 -->
          <div v-for="(item,index) in hotComment" :key="index" class="item">
            <div class="icon-wrap">
              <!-- 用户评论头像-->
              <img :src="item.user.avatarUrl" alt="" />
            </div>
            <div class="content-wrap">
              <div class="content">
                <!-- 昵称 -->
                <span class="name">{{ item.user.nickname }}：</span>
                <span class="comment">{{ item.content }}</span>
              </div>
              <!-- 评论的回复-->
              <div v-if="item.beReplied.length!=0" class="re-content">
                <span class="name">{{ item.beReplied[0].user.nickname}}：</span>
                <span class="comment">{{ item.beReplied[0].content }}</span>
              </div>
              <div class="date">2020-02-12 17:26:11</div>
            </div>
          </div>
        </div>
      </div>
      <!-- 最新评论 -->
      <div class="comment-wrap">
        <p class="title">最新评论<span class="number">(666)</span></p>
        <div class="comments-wrap">
          <div class="item">
            <div class="icon-wrap">
              <img src="../assets/avatar.jpg" alt="" />
            </div>
            <div class="content-wrap">
              <div class="content">
                <span class="name">爱斯基摩：</span>
                <span class="comment">谁说的，长大了依旧可爱哈</span>
              </div>
              <div class="re-content">
                <span class="name">小苹果：</span>
                <span class="comment">还是小时候比较可爱</span>
              </div>
              <div class="date">2020-02-12 17:26:11</div>
            </div>
          </div>
          <div class="item">
            <div class="icon-wrap">
              <img src="../assets/avatar.jpg" alt="" />
            </div>
            <div class="content-wrap">
              <div class="content">
                <span class="name">爱斯基摩：</span>
                <span class="comment">谁说的，长大了依旧可爱哈</span>
              </div>
              <div class="re-content">
                <span class="name">小苹果：</span>
                <span class="comment">还是小时候比较可爱</span>
              </div>
              <div class="date">2020-02-12 17:26:11</div>
            </div>
          </div>
          <div class="item">
            <div class="icon-wrap">
              <img src="../assets/avatar.jpg" alt="" />
            </div>
            <div class="content-wrap">
              <div class="content">
                <span class="name">爱斯基摩：</span>
                <span class="comment">谁说的，长大了依旧可爱哈</span>
              </div>
              <div class="re-content">
                <span class="name">小苹果：</span>
                <span class="comment">还是小时候比较可爱</span>
              </div>
              <div class="date">2020-02-12 17:26:11</div>
            </div>
          </div>
        </div>
      </div>
      <!-- 分页器 -->
      <el-pagination
        @current-change="handleCurrentChange"
        background
        layout="prev, pager, next"
        :total="total"
        :current-page="page"
        :page-size="5"
        :limit="limit"
      >
      </el-pagination>
    </div>
    <div class="mv-recommend">
      <h3 class="title">相关推荐</h3>
      <div class="mvs">
        <div class="items">
          <div v-for="(item,index) in simiMvs" :key="index" class="item" @click="toMV(item.id)">
            <div class="img-wrap">
              <img :src="item.cover" alt="" />
              <span class="iconfont icon-play"></span>
              <div class="num-wrap">
                <div class="iconfont icon-play"></div>
                <div class="num">{{item.playCount}}</div>
              </div>
              <span class="time">{{item.duration}}</span>
            </div>
            <div class="info-wrap">
              <div class="name">{{item.name}}</div>
              <div class="singer">{{item.artistName}}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
export default {
  name: 'mv',
  data() {
    return {
      // 总条数
      total: 20,
      // 页码
      page: 1,
      // 页容量
      limit: 10,
        //mv地址
      url:"",
        //相关mv
        simiMvs:[],
        //mv的信息
        mvInfo:{},
        // 头像
        icon:'',
        // 热门评论
        hotComment:[],
        // 热门评论的个数
        hotCount:0,
        // 普通评论
        comments:[]
    };
  },
    
    created() {
      // 获取mv播放地址
      axios({
          url:'https://autumnfish.cn/mv/url',
          method:'get',
          params:{
              // 获取url中的 id
              id:this.$route.query.q
          }
      }).then(res=>{
          this.url = res.data.data.url
      })
        // 获取相关的mv
        axios({
            url: 'https://autumnfish.cn/simi/mv',
            method: 'get',
            params: {
                mvid: this.$route.query.q
            }
        }).then(res => {
            // 保存相关MV
            this.simiMvs = res.data.mvs
            // 处理播放次数
            for (let i = 0; i < this.simiMvs.length; i++) {
                if (this.simiMvs[i].playCount > 100000) {
                    this.simiMvs[i].playCount = parseInt(this.simiMvs[i].playCount / 10000) + '万'
                }
            }
            // 计算歌曲时间
            for (let i = 0; i < this.simiMvs.length; i++) {
                let min = parseInt(this.simiMvs[i].duration / 1000 / 60)
                let sec = parseInt(this.simiMvs[i].duration / 1000 % 60)
                if (min < 10) {
                    min = '0' + min
                }
                if (sec < 10) {
                    sec = '0' + sec
                }
                this.simiMvs[i].duration = min + ':' + sec
            }
        })
        // 获取mv的信息
        axios({
            url:'https://autumnfish.cn/mv/detail',
            method:'get',
            params:{
                mvid:this.$route.query.q
            }
        }).then(res=>{
            //mv的信息
            this.mvInfo = res.data.data
            // 获取歌手信息
            axios({
                url:'https://autumnfish.cn/artists',
                method:'get',
                params:{
                    id:this.mvInfo.artists[0].id
                }
            }).then(res=>{
                // 歌手的封面信息
                this.icon = res.data.artist.picUrl
            })
        })
        
        // 获取评论的数据
        axios({
            url:'https://autumnfish.cn/comment/mv',
            method:'get',
            params:{
                id:this.$route.query.q,
                limit:20,
                offset:0
            }
        }).then(res=>{
            this.hotComment = res.data.hotComments
            //保存个数
            this.hotCount = res.data.total
            // 总个数
            this.total = res.data.total
            // 评论数据
            this.comments = res.data.comments
        })
    },
    methods: {
    // 去mv详情页面
    toMV(id){
        this.$router.push(`/mv?q=${id}`)
        this.$router.go(0)//刷新当前页面
    },
    handleCurrentChange(val) {
      //console.log(`当前页: ${val}`);
        // 保存页码
        this.page = val
        // 重新获取数据
        axios({
            url:'https://autumnfish.cn/comment/mv',
            method:'get',
            params:{
                id:this.$route.query.q,
                limit:10,
                offset:(this.page-1)*20
            }
        }).then(res=>{
            // 总个数
            this.total = res.data.total
            // 评论数据
            this.comments = res.data.comments
        })
    }
  }
};
</script>

<style></style>

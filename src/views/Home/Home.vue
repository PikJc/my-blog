<template>
  <div class="home">
    <div :style="bannerClass" class="banner">
       <div class="attach">
         <div class="attach-info">
           <div class="info-title">
             <span>{{subTitle}}</span>
           </div>
           <ul>
             <li>
               <el-button icon="el-icon-arrow-left" type="text" @click="onleft"></el-button>
             </li>
             <li v-for="(item,key) in contactList" :key="key">
               <a :href="item.link" target="_blank">
                 <img :src="item.icon"  class="icon-font">
               </a>
               <div v-if="item.img" class="code">
                 <img :src="item.img">
               </div>
             </li>
             <li>
               <el-button icon="el-icon-arrow-right" type="text" @click="onright"></el-button>
             </li>
           </ul>
         </div>
       </div>
    </div>
    <div class="content">
      <v-banner class="mt-4 content-warning" elevation="0.8" single-line >
        <v-icon
          class="content-warning-icon"
          slot="icon"
          color="warning"
          size="36"
        >
          mdi-bell-ring
        </v-icon>
        {{notify}}
      </v-banner>
      <Article :articleList="articleList" :number="totalCount"/>
      <v-btn color="error" class="mt-4 mb-8" @click="gotoIndex">More<v-icon>mdi-plus</v-icon></v-btn>
      <div style="position:absolute;left: -400px;top: 100px;display: flex;flex-direction: column">
        <img src="../../assets/images/caihong.png" alt="" width="200">
        <span class="mt-8" style="font-size: 32px;color: #409eff;">在你左边画一条彩虹</span>
      </div>
      <div style="position:absolute;right: -400px;top: 100px;display: flex;flex-direction: column">
        <img src="../../assets/images/long.png" alt="" width="200">
        <span class="mt-8" style="font-size: 32px;color: #d94f3e;">再在右边画条龙</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import config from '@/config'
import Article from '../../components/Article/Index.vue'
import { queryPosts, queryArchivesCount } from '@/utils/services'
import { formatPost } from '@/utils/format'
@Component({
  components: {
    Article
  }
})
export default class Home extends Vue {
  contactList:any[] = config.contact
  notify:string = config.notify
  totalCount = ''
  articleList = []
  searchDTO:any = {
    page: 1,
    pageSize: 6,
    filter: ''
  }
  bannerImg:any[] = []
  random:number = 0
  bannerClass:any = {}
  subTitle:string = config.subtitle

  created () {
    this.bannerImg = [
      'https://picsum.photos/id/171/2048/1536',
      'https://picsum.photos/id/179/2048/1365',
      'https://picsum.photos/id/183/2316/1544',
      'https://picsum.photos/id/209/1920/1280',
      'https://picsum.photos/id/212/2000/1394'
    ]
  }

  async mounted () {
    this.totalCount = await queryArchivesCount()
    let result = await queryPosts(this.searchDTO)
    formatPost(result)
    this.articleList = result
    this.random = Math.floor(Math.random() * this.bannerImg.length)
    this.bannerClass = {
      'background-image': 'url(' + this.bannerImg[this.random] + ')',
      'background-size': 'cover',
      'background-attachment': 'fixed'
    }
  }

  onleft () {
    if (this.random === 0) {
      this.random = this.bannerImg.length - 1
    } else {
      this.random = this.random - 1
    }
    this.bannerClass['background-image'] = 'url(' + this.bannerImg[this.random] + ')'
  }
  onright () {
    let num = this.random + 1
    if (num === this.bannerImg.length) {
      this.random = 0
    } else {
      this.random = num
    }
    this.bannerClass['background-image'] = 'url(' + this.bannerImg[this.random] + ')'
  }
  gotoIndex () {
    this.$router.push('/home')
  }
}
</script>

<style scoped lang="less">
  @import "./home.less";
</style>

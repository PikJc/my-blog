<template>
  <header class="my-blog-header" :class="isTop? '' : 'header-background '">
    <div style="height: 100%;width: 200px">
      <router-link to="/" class="logo">
          <img src="../../assets/logo.png" style="height: 40px">
          <span class="logo-title" :style="{ color: isTop ? '' : '#2c3e50'}">{{logoTitle}}</span>
      </router-link>
    </div>
    <nav class="menu">
      <ul>
        <li class="nav-item" v-for="(item,key) in menu" :key="key">
          <router-link :to="item.url">
            <a>
              <i class="iconfont" :class="item.icon" style="font-size: 20px;margin-right: 4px"></i>
              <span>{{item.name}}</span>
            </a>
          </router-link>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script lang="ts">
import { Vue, Component, Prop, Model, Watch } from 'vue-property-decorator'
import config from '@/config/index'
@Component({})
export default class Index extends Vue {
  menu:any[] = config.menu;
  isTop:boolean = false;
  logoTitle:string = config.title;

  created () {
    this.handleTop()
    window.addEventListener('scroll', this.handleTop)
  }
  // 处理滚动事件
  handleTop () {
    const top = document.documentElement.scrollTop || document.body.scrollTop
    if (!top) {
      this.isTop = true
    } else {
      this.isTop = false
    }
    // 判断位置，控制滚动到顶部
  }
}
</script>

<style scoped lang="less">
  @import "./index.less";
</style>

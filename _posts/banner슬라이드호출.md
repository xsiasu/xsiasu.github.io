---
title: How to Get Started
author: Tao He
date: 2019-04-28
category: Jekyll
layout: post
---

# 슬라이드 베너를 적용하는 기본방법 
# 데이터를 api를 통해서 호출
```vue:banner.json
[
    {
        "title":"메인상품1",
        "subtitle":"메인상품설명1",
        "image":"https://picsum.photos/1920/570"
    },
    {
        "title":"메인상품2",
        "subtitle":"메인상품설명2",
        "image":"https://picsum.photos/1920/570"
    },
    {
        "title":"메인상품3",
        "subtitle":"메인상품설명3",
        "image":"https://picsum.photos/1920/570"
    }        
]
```
```vue:banner.js
import http from "./http";
//aync es6 에 promise 가 좀 더  편하게 바뀌었다고 생각하면 됨
export default {
  async getMainSlideBanners() {
    return http.get("api/banner.json");
  },
};
```
```vue:SlideVue.vue
<template>
  <section class="slide1">
    <div class="wrap-slick1">
      <div ref="slick" class="slick1">
        <template v-for="banner in banners">
          <div
            :key="banner.title"
            class="item-slick1 item1-slick1"
            :style="`background-image: url(${banner.image});`"
          >
            <div
              class="wrap-content-slide1 sizefull flex-col-c-m p-l-15 p-r-15 p-t-150 p-b-170"
            >
              <span
                class="caption1-slide1 m-text1 t-center animated visible-false m-b-15"
                data-appear="fadeInDown"
              >
                {{ banner.subtitle }}
              </span>

              <h2
                class="caption2-slide1 xl-text1 t-center animated visible-false m-b-37"
                data-appear="fadeInUp"
              >
                {{ banner.title }}
              </h2>

              <div
                class="wrap-btn-slide1 w-size1 animated visible-false"
                data-appear="zoomIn"
              >
                <!-- Button -->
                <a
                  href="product.html"
                  class="flex-c-m size2 bo-rad-23 s-text2 bgwhite hov1 trans-0-4"
                >
                  Shop Now
                </a>
              </div>
            </div>
          </div>
        </template>
      </div>
    </div>
  </section>
</template>

<script>
import bannerApi from '@/api/banner'
export default {
  data() {
    return {
    //banner 배열 선언
      banners : []
    };
  },
  created(){
    bannerApi.getMainSlideBanners().then((response)=> {
      this.banners = [].concat(response.data)

      //slick jquery plugin 적용
      // $nextTick() 메쏘드가 활용
      this.$nextTick(()=> {
        $(this.$refs.slick).slick1()
      })
      
    })
  }
};  
</script>
```

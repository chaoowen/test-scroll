<template>
  <div ref="container" id="app">
    <div class="title">
      <p
        data-scroll :data-scroll-speed="random(5, 15)"
        v-for="(letter, index) in title" :key="index">
        {{ letter }}
      </p>
    </div>
    <section class="scene-one">
      <header
        data-scroll data-scroll-sticky data-scroll-target=".scene-one"
        class="headerFixed"
      >
        Let's have some ___ .
      </header>
      <div class="introduction">
        <img
          src="https://memorial.fontech.com.tw/static/images/page/fontech-logo-big.svg"
          alt="fontech"
        >
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="-2">我們是 Fontech 方客特</p>
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="2">專業軟體開發商</p>
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="-2">品牌形象都可以量身定制</p>
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="2">更輕鬆的管理、更俐落的上手</p>
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="-2">更具質感的介面、更無痛的數位體驗</p>
      </div>
      <div class="coding">
        <div>
          <img
            data-scroll data-scroll-speed="3"
            src="./assets/images/coding.png" alt="coding"
          >
        </div>
        <p data-scroll data-scroll-offset="400" data-scroll-speed="-1">坐而言，不如起而行</p>
      </div>
    </section>
    <section class="scene-two">
      <div
        class="imagesContent"
        v-for="(image, index) in imageList" :key="index"
      >
        <img
          data-scroll data-scroll-direction="horizontal"
          :data-scroll-offset="image.offset"
          :data-scroll-speed="image.speed"
          :src="image.url" :alt="image.name"
        >
      </div>
    </section>
    <section class="scene-three">
      <img
        data-scroll data-scroll-speed="3"
        src="./assets/images/mountain.jpeg" alt="coding"
      >
    </section>
    <div style="height: 500px"></div>

  </div>
</template>

<script>
import LocomotiveScroll from 'locomotive-scroll'

export default {
  name: 'App',
  mounted () {
    setTimeout(() => {
      // eslint-disable-next-line no-new
      new LocomotiveScroll({
        el: document.getElementById('app'),
        smooth: true,
        lerp: 0.04,
        repeat: true,
        tablet: {
          smooth: true,
          breakpoint: 250
        },
        smartphone: {
          smooth: false
        }
      })
    }, 400)
  },
  data () {
    return {
      title: 'FONTECH',
      imageList: [
        { name: 'html', url: require('./assets/images/html.jpeg'), offset: '100', speed: '-1' },
        { name: 'backend', url: require('./assets/images/backend.png'), offset: '200', speed: '1.5' },
        { name: 'frontend', url: require('./assets/images/frontend.jpeg'), offset: '200', speed: '-2' },
        { name: 'cicd', url: require('./assets/images/cicd.png'), offset: '300', speed: '1.5' }
      ]
    }
  },
  methods: {
    random (max, min) {
      return Math.random() * (max - min) + min
    }
  }

}
</script>

<style>
#app, html, body {
  padding: 0;
  margin: 0;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  /* -webkit-font-smoothing: antialiased; */
  /* -moz-osx-font-smoothing: grayscale; */
  text-align: center;
  color: #2c3e50;
}

.title {
  display: flex;
  justify-content: center;
  margin-top: 60px;
  font-size: 200px;
  font-weight: 700;
}
.title p {
  padding: 0 20px;
}

.headerFixed {
  position: relative;
  padding: 40px;
  color: white;
  font-size: 40px;
  font-weight: 700;
  background-color: black ;
  z-index: 10;
}

.introduction {
  margin: 100px;
}
.introduction p {
  padding: 20px 0;
  font-size: 20px;
  font-weight: 600;
}
.introduction img {
  margin-bottom: 100px;
}

.coding {
  position: relative;
  width: 100vw;
}
.coding div {
  width: 800px;
  height: 500px;
}
.coding img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
.coding p {
  opacity: 0;
  transition: opacity .7s ease;

  position: absolute;
  top: 0;
  margin: 0;
  color: #ffe300;
  text-align: center;
  font-size: 60px;
  font-weight: 700;
  letter-spacing: 10px;
}
.coding p.is-inview {
  opacity: 1;
}

.scene-two {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  grid-gap: 20px;
  margin: 60px 0 200px 0;
}
.imagesContent {
  width: 400px;
  height: 200px;
  align-self: center;
  justify-self: center;
}
.imagesContent img {
  width: 100%;
  height: 100%;
  object-fit: contain;

  opacity: 0;
  transition: opacity .7s ease;
}
.scene-two .imagesContent img.is-inview {
  opacity: 1;
}
</style>

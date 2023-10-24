<template>
  <div ref="container" id="app">
    <!-- <section
      data-scroll data-scroll-sticky data-scroll-target="#app" data-scroll-position="bottom"
      style="position: absolute; width: 100vw; height: 100vh; z-index: 20"
    >
      <div
        class="scrollTopFixed"
        @click="handleGoTop"
      >
        GO TOP
      </div>
    </section> -->

    <section>
      <div class="title">
        <p
          data-scroll :data-scroll-speed="random(5, 15)"
          data-scroll-position="top" data-scroll-delay="0.05"
          v-for="(letter, index) in title" :key="index">
          {{ letter }}
        </p>
      </div>
    </section>

    <section class="scene-one">
      <header
        data-scroll data-scroll-sticky data-scroll-target=".scene-one" data-scroll-position="top"
        class="headerFixed"
      >
        Let's have some ___ .
      </header>

      <div class="introduction">
        <img
          src="https://memorial.fontech.com.tw/static/images/page/fontech-logo-big.svg"
          alt="fontech"
        >
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="-10">我們是 Fontech 方客特</p>
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="7">專業軟體開發商</p>
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="-8">品牌形象都可以量身定制</p>
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="9">更輕鬆的管理、更俐落的上手</p>
        <p data-scroll data-scroll-direction="horizontal" data-scroll-speed="-5">更具質感的介面、更無痛的數位體驗</p>
      </div>

      <div class="coding">
        <div>
          <img
            data-scroll data-scroll-speed="0.5" data-scroll-offset="-200"
            src="./assets/images/mountain.jpeg" alt="coding"
          >
        </div>
        <p data-scroll data-scroll-speed="-0.5">坐而言，不如起而行</p>
      </div>
    </section>

    <section>
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

      <!-- wave words -->
      <div class="wave">
        <p
          v-for="(word,index) in waveWords" :key="word"
          data-scroll :data-scroll-delay="waveMath(index)" data-scroll-speed="4"
        >
          {{ word }}
        </p>
      </div>

      <!-- jumping rabbits -->
      <section class="rabbits">
        <div
          v-for="(item, index) in 3" :key="index"
          :class="['jumpingImage', setRabbitSize(index)]"
          @mouseover="hoverRabbitIndex = index"
          @mouseleave="hoverRabbitIndex = 1"
        >
          <img
            :class="[setRabbitSize(index), { rabbitJump: hoverRabbitIndex === index }]"
            src="./assets/images/rabbit.png" alt="rabbit"
          >
          <div :class="setRabbitSize(index)"></div>
        </div>
      </section>

    </section>

  </div>
</template>

<script>
import LocomotiveScroll from 'locomotive-scroll'

export default {
  name: 'App',
  mounted () {
    // eslint-disable-next-line no-new
    this.scroll = new LocomotiveScroll({
      el: document.getElementById('app'),
      smooth: true,
      lerp: 0.08,
      repeat: true,
      tablet: {
        smooth: true,
        breakpoint: 250
      },
      smartphone: {
        smooth: false
      }
    })
    this.scroll.on('call', (func, state, event) => {
      switch (func) {
        case 'setNumber':
          if (state === 'enter') {
            this.handleSetNumber()
          } else {
            this.handleResetNumber()
          }
      }
    })
    setTimeout(() => {
      this.scroll.update()
    }, 1000)
  },
  data () {
    return {
      scroll: null,
      title: 'FONTECH',
      imageList: [
        { name: 'html', url: require('./assets/images/html.jpeg'), offset: '100', speed: '-1' },
        { name: 'backend', url: require('./assets/images/backend.png'), offset: '200', speed: '1.5' },
        { name: 'frontend', url: require('./assets/images/frontend.jpeg'), offset: '200', speed: '-2' },
        { name: 'cicd', url: require('./assets/images/cicd.png'), offset: '300', speed: '1.5' }
      ],
      number: {
        year: 1,
        month: 1,
        date: 1
      },
      hoverRabbitIndex: 1,
      waveWords: '利用時間延遲做出波浪字體吧'
    }
  },
  watch: {
    hoverRabbitIndex () {
      this.scroll.update()
    }
  },
  methods: {
    setRabbitSize (index) {
      return this.hoverRabbitIndex === index ? 'largeRabbitSize' : 'smallRabbitSize'
    },
    random (max, min) {
      return Math.random() * (max - min) + min
    },
    waveMath (index) {
      return (15 - index) * 0.01
    },
    handleGoTop () {
      const target = document.getElementById('app')
      this.scroll.scrollTo(target)
    },
    handleResetNumber () {
      this.number.year = 1
      this.number.month = 1
      this.number.date = 1
    },
    handleSetNumber () {
      const numberInterval = setInterval(() => {
        if (this.number.year < new Date().getFullYear() - 1911) {
          this.number.year++
        }
        if (this.number.month < new Date().getMonth() + 1) {
          this.number.month++
        }
        if (this.number.date < new Date().getDate()) {
          this.number.date++
        }
        if (this.number.year === 102 && this.number.month === 12 && this.number.date === 30) {
          clearInterval(numberInterval)
        }
      }, 50)
    }
  }

}
</script>

<style>
html, body {
  padding: 0;
  margin: 0;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  /* -webkit-font-smoothing: antialiased; */
  /* -moz-osx-font-smoothing: grayscale; */
  text-align: center;
  color: #2c3e50;
  display: flex;
}
#app {
  position: relative;
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

.scrollTopFixed {
  display: inline-block;
  /* position: sticky; */
  position: relative;
  z-index: 20;
  /* position: fixed;
  bottom: 20px;
  right: 20px; */

  padding: 16px;
  color: white;
  font-size: 20px;
  font-weight: 500;
  background-color: green ;
  cursor: pointer;
  border-radius: 16px;
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

.introduction p {
  padding: 20px 0;
  font-size: 24px;
  font-weight: 500;
}
.introduction img {
  padding-bottom: 100px;
}

.coding {
  /* position: relative; */
  width: 100vw;

  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 800px; /* 父元素的高度 */
}
.coding div {
  width: 100vw;
  height: 800px;
}
.coding img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: all 10s ease;
  animation: zoom-out 5s forwards;
}
.coding img.is-inview {
  animation: zoom-in 5s forwards;
}
@keyframes zoom-in {
  0% {
    width: 100%;
    height: 100%;
  }
  100% {
    width: 120%;
    height: 100%;
  }
}
@keyframes zoom-out {
  0% {
    width: 120%;
    height: 100%;
  }
  100% {
    width: 100%;
    height: 100%;
  }
}
.coding p {
  position: absolute;

  margin: 0;
  color: #ffe300;
  text-align: center;
  font-size: 60px;
  font-weight: 700;
  letter-spacing: 10px;
}

.scene-two {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  grid-gap: 20px;
  padding: 160px;
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
/* --------- rabbit --------- */
.smallRabbitSize {
  width: 150px;
  height: 300px;
  transition: all .5s ease-in-out;
}
.largeRabbitSize {
  width: 200px;
  height: 450px;
  transition: all .5s ease-in-out;
}

.rabbits {
  display: flex;
  align-items: center;
  justify-content: center;
}
.rabbitJump {
  animation-name: moveUp;
  animation-duration: .5s;
  animation-delay: .3s;
  animation-fill-mode: forwards;
}
@keyframes moveUp {
  0% {
    top: 0;
  }
  100% {
    top: -20px;
  }
}
.jumpingImage {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 40px;
}
.jumpingImage img {
  position: absolute;
  object-fit: contain;
}
.jumpingImage div {
  border-style: solid;
  border-width: 10px;
  border-color: gray;
}

/* ------- wave ------- */
.wave {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 60px;
}
.wave p {
  padding: 8px;
  font-size: 40px;
  font-weight: 500;
}
</style>

<template>
  <div>
    <h1 id="title" :class="[hover ? hover : '']" ref="title">
      <span class="hiim">Hi, I'm</span><br/>
      <span class="patrick">Patrick</span> <span class="wang">Wang</span>
    </h1>
    <div class="mobileWarning" v-if="isMobile">
      I look much better on a computer!
    </div>
    <div id="sideMenuIcon" :class="[hover ? hover : '']" @click="showSideMenu=!showSideMenu">
      <ThreeLinesIcon :class="['threeLines', showSideMenu ? 'fadeOut' : '']"/>
      <ArrowIcon :class="['arrow', showSideMenu ? '' : 'fadeOut']"/>
    </div>
    <transition name="sidemenu">
      <div v-if="showSideMenu" id="sideMenu">
        <div v-for="option in sideMenuOptions" :ref="option.id" :key="option.text" :class="['sideMenuOption', hover ? hover : '', hover === 'colorsseum' ? option.colorsseumColor : '']" @click="$router.push(option.path)" @mouseover="sideMenuHover(option.id, true)" @mouseleave="sideMenuHover(option.id, false)">
          {{ option.text }}
        </div>
      </div>
    </transition>
    <div id="plaqueWall">
      <div class="plaqueWrapper">
        <img class="plaque" src="../assets/tapestryPlaque.png" @mouseover="setHover('tapestry')" @mouseleave="setHover('')" @click="goToPlaquePage('tapestry')">
        <div :class="['plaqueDesc', hover === 'tapestry' ? hover : '']">
          An educational tool which lets you visualize your code's memory
        </div>
      </div>
      <div class="plaqueWrapper">
        <img class="plaque" src="../assets/colorsseumPlaque.png" @mouseover="setHover('colorsseum')" @mouseleave="setHover('')" @click="goToPlaquePage('colorsseum')">
        <div :class="['plaqueDesc', hover === 'colorsseum' ? hover : '']">
          A platform fighter with abilities like a MOBA
        </div>
      </div>
      <div class="plaqueWrapper">
        <img class="plaque" src="../assets/roboticsPlaque.png" @mouseover="setHover('robotics')" @mouseleave="setHover('')" @click="goToPlaquePage('robotics')">
        <div :class="['plaqueDesc', hover === 'robotics' ? hover : '']">
          A training simulation of the 2018 FIRST Robotics Game
        </div>
      </div>
      <div class="plaqueWrapper">
        <img class="plaque" src="../assets/gdcPlaque.png" @mouseover="setHover('gdc')" @mouseleave="setHover('')" @click="goToPlaquePage('gdc')">
        <div :class="['plaqueDesc', hover === 'gdc' ? hover : '']">
          Cupertino High School's game development club
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ThreeLinesIcon from '@/assets/ThreeLinesIcon.vue'
import ArrowIcon from '@/assets/ArrowIcon.vue'

export default {
  name: 'Home',
  components: {
    ThreeLinesIcon,
    ArrowIcon
  },
  data () {
    return {
      showSideMenu: false,
      hover: null,
      pageColors: {
        tapestry: '#323757',
        colorsseum: '#FFDBFF',
        gdc: '#525252',
        robotics: '#AE4C45'
      },
      sideMenuOptions: [
        {
          text: 'Text Wall',
          id: 'textWall',
          colorsseumColor: 'green',
          path: 'textwall'
        },
        {
          text: 'Connect',
          id: 'connect',
          colorsseumColor: 'blue',
          path: 'connect'
        }
      ]
    }
  },
  computed: {
    isMobile () {
      const toMatch = [
        /Android/i,
        /webOS/i,
        /iPhone/i,
        /iPad/i,
        /iPod/i,
        /BlackBerry/i,
        /Windows Phone/i
      ]

      return toMatch.some((toMatchItem) => {
        return navigator.userAgent.match(toMatchItem)
      })
    }
  },
  methods: {
    sideMenuHover (ref, state) {
      const element = this.$refs[ref][0]

      const oldTransition = element.style.transition
      element.style.transition = 'none'

      if (state === true) {
        element.style.color = 'white'
        element.style.backgroundColor = 'black'
      } else if (state === false) {
        element.style.color = null
        element.style.backgroundColor = null
      }

      /* eslint-disable-next-line */
      const trash = element.offsetHeight
      element.style.transition = oldTransition
    },
    setHover (newHover) {
      if (this.$router.currentRoute.path === '/') { // on Safari it calls setHover('') even when you go to a different page
        this.hover = newHover

        if (this.hover === '') {
          document.body.style.backgroundColor = 'white'
        } else {
          document.body.style.backgroundColor = this.pageColors[this.hover]
        }
      }
    },
    goToPlaquePage (path) {
      this.$router.push(path)
    }
  },
  created () {
    const oldTransition = document.body.style.transition
    document.body.style.transition = 'none'
    document.body.style.backgroundColor = 'white'
    /* eslint-disable-next-line */
    const trash = document.body.offsetHeight
    document.body.style.transition = oldTransition
  }
}
</script>

<style scoped>
#title {
  text-align: center;
  font-size: 5.1vw;
  margin-bottom: 1vw;
  color: black;
}
#title > span {
  transition: color 0.5s;
}
.mobileWarning {
  text-align: center;
  font-size: 3vw;
  margin-bottom: 1vw;
  color: black;
}
#sideMenuIcon {
  position: fixed;
  top: 1.7vw;
  left: 1.7vw;
  z-index: 1;
  cursor: pointer;
}
.threeLines {
  position: absolute;
  width: 2.6vw;
  height: 2.6vw;
  transform: rotate(0deg);
  transition: transform 0.13s ease-out 0.13s, opacity 0.13s ease-out 0.13s, fill 0.5s;
}
.threeLines.fadeOut {
  opacity: 0;
  transform: rotate(90deg);
  transition: transform 0.13s ease-in, opacity 0.13s ease-in, fill 0.5s;
}
.arrow {
  position: absolute;
  width: 2.6vw;
  height: 2.6vw;
  transform: rotate(180deg);
  transition: transform 0.13s ease-out 0.13s, opacity 0.13s ease-out 0.13s, fill 0.5s;
}
.arrow.fadeOut {
  opacity: 0;
  transform: rotate(90deg);
  transition: transform 0.13s ease-in, opacity 0.13s ease-in, fill 0.5s;
}
#sideMenu {
  position: fixed;
  top: 0;
  left: 0p;
  height: 100%;
  width: 20vw;
  background: transparent;
  padding-top: 5vw;
}
.sidemenu-enter-active, .sidemenu-leave-active {
  transition: transform 0.25s;
}
.sidemenu-enter, .sidemenu-leave-to {
  transform: translateX(-22vw);
}
.sideMenuOption {
  width: 20vw;
  font-size: 2.5vw;
  padding: 0.5vw 1.8vw;
  background: transparent;
  border: none;
  text-align: left;
  cursor: pointer;
  transition: color 0.5s;
}
#plaqueWall {
  display: flex;
  align-items: center;
  flex-direction: column;
  flex-wrap: wrap;
}
.plaqueTemp {
  text-align: center;
  font-size: 2.5vw;
  width: 45vw;
  padding: 1.5vw 0;
  margin: 1vw 0.5vw;
  cursor: pointer;
}
.plaqueWrapper {
  position: relative;
}
.plaque {
  cursor: pointer;
  width: 45vw;
  margin: 1vw 0.5vw;
}
.plaqueDesc {
  text-align: left;
  position: absolute;
  right: -0.5vw;
  top: 2vw;
  transform: translateX(100%);
  width: 10vw;
  color: transparent;
  font-size: 1.1vw;
  transition: color 0.5s;
}

.plaqueDesc.tapestry {
  color: #A67B25;
}
.plaqueDesc.colorsseum {
  color: #1DB660;
}
.plaqueDesc.gdc {
  color: #FFFFFF;
}
.plaqueDesc.robotics {
  color: #E99791;
}
#title.tapestry {
  color: #A67B25;
}
#sideMenuIcon.tapestry > svg {
  fill: #A67B25;
}
.sideMenuOption.tapestry {
  color: #A67B25;
}
#title.colorsseum > .hiim {
  color: #CA9B00;
}
#title.colorsseum > .patrick {
  color: #1DB660;
}
#title.colorsseum > .wang {
  color: #2C95D1;
}
#sideMenuIcon.colorsseum > svg {
  fill: #CA9B00;
}
.sideMenuOption.orange {
  color: #CA9B00;
}
.sideMenuOption.green {
  color: #1DB660;
}
.sideMenuOption.blue {
  color: #2C95D1;
}
#title.gdc {
  color: #FFFFFF;
}
#sideMenuIcon.gdc > svg {
  fill: #FFFFFF;
}
.sideMenuOption.gdc {
  color: #FFFFFF;
}
#title.robotics {
  color: #E99791;
}
#sideMenuIcon.robotics > svg {
  fill: #E99791;
}
.sideMenuOption.robotics {
  color: #E99791;
}
</style>

<style>
body {
  transition: background-color 0.5s;
  margin: 0;
}
</style>

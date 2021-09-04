<template>
  <div class="home">
    <Layout>
      <div class="main" @mousemove="mouseMove">
        <div class="container" id="container">
          <div v-for="edge in $static.allPost.edges" :key="edge.node.id">
            <div
              v-on:mouseover="
                changeURL(edge.node.cover_image, edge.node.Description10)
              "
            >
              <g-link class="hover-title" :to="edge.node.path">
                <!-- <span>{{ hover ? '⧈' : '⧠' }}</span> -->
                ⧠
                <span
                  class="spantitle"
                  @mouseover="hover = true"
                  @mouseleave="hover = false"
                  >{{ edge.node.title }}</span
                >
              </g-link>
            </div>
            <!-- <g-image class="hover-image" :src="edge.node.cover_image"></g-image> -->
          </div>
        </div>
      </div>
      <!-- <Foofoo></Foofoo> -->
      <!-- <div class="header_element header2 round-button">
        <g-link class="nav__link" to="/about/">About</g-link>
      </div>-->

      <g-link ref="aboutBar" class="nav__link" to="/about/">
        <div class="sideheader">
          <div class="sideheader-item sideheader1">ABOUT</div>
          <div class="sideheader-item sideheader2"></div>
        </div>
      </g-link>
      <coverimage
        id="img_block"
        v-bind:url="currentURL"
        v-show="currentURL != ''"
      />
      <div id="shortDes">{{ currentDescription }}</div>
    </Layout>
  </div>
</template>

<static-query>
{
 allPost(sortBy:"order"){
  edges{
    node{
      content
      title
      Description10
      Description50
      year
      path
      cover_image 
    }
  }
}
}
</static-query>

<script>
import Foofoo from "../components/Footer";
import coverimage from "../components/Cover";
import { gsap, Power2 } from "gsap";
import { TimelineLite } from "gsap";

export default {
  metaInfo: {
    title: "Work",
  },
  components: {
    Foofoo,
    coverimage,
  },
  data() {
    return {
      currentURL: "",
      currentDescription: "A creative technologist, developer and art director",
      hover: false,
    };
  },
  methods: {
    changeURL: function (url, des) {
      this.currentURL = url.src;
      this.currentDescription = des;
    },
    fillcube: function () {
      console.log(this.$refs.cuberef.innerText);
      if (this.$refs.cuberef.innerText == "⧈") {
        this.$refs.cuberef.innerText = "⧠";
      } else {
        this.$refs.cuberef.innerText = "⧈";
      }
    },
    mouseMove(event) {
      const img = document.getElementById("img_block");
      const box = document.getElementById("container");
      if (
        event.x > box.getBoundingClientRect().left &&
        box.getBoundingClientRect().right &&
        event.y > box.getBoundingClientRect().top &&
        event.y < box.getBoundingClientRect().bottom
      ) {
        var xPos = event.x - img.offsetWidth / 2;
        var yPos = event.y - img.offsetHeight / 2;
        gsap.to(img, 1, {
          x: xPos,
          y: yPos,
          ease: Power2.easeOut,
        });
      } else {
        this.currentURL = "";
      }
    },
  },
  mounted() {
    console.log("Hello there. Welcome to Lan's site :)");
    const box = document.querySelectorAll(".hover-title");
    gsap.from(box, 1, {
      x: -20,
      opacity: 0,
      stagger: {
        each: 0.1,
        start: "center",
      },
      y: -20,
      ease: Power2.easeOut,
    });
  },
};
</script>

<style>
.main {
  left: 2rem;
  right: 2rem;
  padding-top: 1rem /*!main_margin*/;
  padding-bottom: 1rem /*!main_margin*/;
  padding-left: 1rem /*!main_margin*/;
  padding-right: 1rem /*!main_margin*/;
  margin: 0;
  position: absolute;
  top: 50%;
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
}

.container {
  display: flex;
  flex-wrap: wrap;
  align-items: left;
  justify-content: left;
  padding: 10vh 0;
  font-family: "Rozha One", serif;
}

.sideheader {
  position: fixed;
  right: 0;
  top: 0;
  width: 30px;
  height: 100%;
  writing-mode: vertical-lr;
  text-orientation: sideways;
  line-height: 0.5em;
  display: flex;
  padding: 0em;
  transition: all 0.3s ease;
}

.sideheader:hover {
  width: 50px;
}

.sideheader-item {
  height: 50%;
}

.sideheader1 {
  background-color: #ffb200;
  padding: 0.5em;
}
.sideheader2 {
  background-image: linear-gradient(#e88d39, #ef6969, #46c8c0, #c2d7ff);
}

.round-button {
  background-color: #ffb200;
  width: calc(6vw - 2px);
  height: calc(6vw - 2px);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  -webkit-transition: -webkit-transform 0.8s ease-in-out;
  transition: transform 0.8s ease-in-out;
}

.round-button:hover {
  -webkit-transform: rotate(360deg);
  transform: rotate(360deg);
}

#shortDes {
  /* background-color: black;
  color: white; */
  font-family: "Poppins", sans-serif;
}

.header2 {
  position: fixed;
  right: 0px;
  bottom: 0;
  padding: var(--world-padding);
  font-size: var(--reg-font-size);
  z-index: 200;
}

.hover-title {
  margin: 10px;
  font-size: 55px;
  font-weight: 400;
  display: inline;
  pointer-events: auto;
  text-decoration: none;
  color: var(--global-font-color);
}

.hover-title:hover {
}

.spantitle:hover {
  animation: fontchange 0.5s ease forwards;
}

.spantitle {
  padding: 0px 20px 0px 20px;
}

@keyframes fontchange {
  0% {
    color: rgba(0, 0, 0, 1);
  }
  25% {
    color: rgba(173, 205, 255, 1);
    background-color: black;
  }
  50% {
    color: rgba(173, 205, 255, 1);
    border-radius: 15px;
  }
  100% {
    color: rgba(255, 255, 255, 1);
    background-color: black;
    border-radius: 15px;
  }
}

#shortDes {
  position: fixed;
  font-size: 20px;
}

@media only screen and (max-width: 600px) {
  #shortDes {
    display: none;
    background: coral;
  }

  .container {
    display: flex;
    flex-wrap: wrap;
    align-items: left;
    justify-content: left;
    padding: 10vh 0;
  }

  .main {
    left: 0rem;
    right: 1rem;
  }

  .round-button {
    background-color: transparent;
    display: inherit;
    width: inherit;
    height: inherit;
    border-radius: 100%;
    font-size: 22px;
    animation: none;
    -webkit-transition: -webkit-transform 0s ease-in-out;
    transition: transform 0s ease-in-out;
  }

  .hover-title {
    font-size: 25px;
    font-weight: 300;
    display: inline;
    pointer-events: auto;
    cursor: pointer;
    text-decoration: none;
    color: var(--global-font-color);
    margin: 0 20px 0 0;
  }

  .spantitle {
    font-size: 25px;
    padding: 0px;
  }

  .spantitle:hover {
    animation: fontchange 0s ease forwards;
  }
}
</style>

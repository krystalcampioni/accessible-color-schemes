<template>
  <div id="app" v-if="selectedColorScheme">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <HelloWorld :primaryColor="primaryTextColor"></HelloWorld>

    <div class="controls">
      <input type="color" v-model="mainColor" @change="updateColors">
      <div class="color-scheme color-scheme--complementary">
        <h2>{{ selectedColorScheme.name }} Colors</h2>
        <ul>
          <li v-for="n in 4" :key="n">some text</li>
        </ul>
      </div>
      <div class="controls__buttons">
        <button
          v-for="scheme in colorSchemes"
          @click="selectColorScheme(scheme)"
          :key="scheme.name"
          :class="{
            '-is-selected': selectedColorScheme === scheme
          }"
        >
          {{ scheme.name }}
        </button>
      </div>
    </div>

  </div>
</template>

<script>
import Color from "color";
import contrast from "get-contrast";

import HelloWorld from './components/HelloWorld';

export default {
  data() {
    return {
      mainColor: "#00fbc8",
      colorSchemes: {
        complementary: {
          name: "Complementary",
          angles: [0, 180, 180]
        },
        splitComplementary: {
          name: "Split Complementary",
          angles: [0, 60, 150]
        },
        triadic: {
          name: "Triadic",
          angles: [0, 120, 240]
        },
        square: {
          name: "Square",
          angles: [90, 180, 270]
        },
        analogous: {
          name: "Analogous",
          angles: [30, 60, 90]
        }
      },
      selectedColorScheme: null
    };
  },
  components: {
    HelloWorld,
  },
  computed: {
    primaryBgColor() {
      let bgColor = Color(this.mainColor);
      // console.log(
      //   bgColor.contrast(Color(this.primaryTextColor))
      // )
      // if () {
        if (bgColor.isLight()) {
          return "white";
          // return this.darkenText(bgColor, this.primaryTextColor);
        }
        if (bgColor.isDark()) {
          return "black";
          // return this.lightenText(bgColor, this.primaryTextColor);
        }
      // }

      return bgColor.string();
    },
    primaryTextColor() {
      const bgColor = Color(this.primaryBgColor);
      let textColor = Color(this.primaryBgColor);

      if (bgColor.isLight()) {
        console.log('is Light')
        textColor = this.darkenText(textColor, bgColor);
      } else {
        console.log('is Dark')
        textColor = this.lightenText(textColor, bgColor);
      }

      return textColor.string();
    },
    primaryContrast() {
      // const color = Color(this.primaryBgColor);
      // const color2 = Color(this.primaryTextColor);
      return contrast.score(this.primaryBgColor, this.primaryTextColor);
      // return null
    },
    secondaryBgColor() {
      const color = Color(this.primaryBgColor);
      return color.rotate(this.selectedColorScheme.angles[0]);
    },
    tertiaryBgColor() {
      const color = Color(this.primaryBgColor);
      return color.rotate(this.selectedColorScheme.angles[1]);
    },
    quaternaryBgColor() {
      const color = Color(this.primaryBgColor);
      return color.rotate(this.selectedColorScheme.angles[2]).string();
    },
  },
  methods: {
    selectColorScheme(scheme) {
      this.selectedColorScheme = scheme;
      this.updateColors();
    },
    updateColorVariable(colorName, newValue) {
      document.documentElement.style.setProperty(colorName, newValue);
    },
    darkenText(color1) {
      let temp = Color(color1);
      let contrast = temp.contrast(color1);

      while (contrast < 5 && temp.hex() !== "#FFFFFF" && temp.hex() !== "#000000") {
        temp = temp.darken(.1);
        contrast = temp.contrast(color1);
      }
      return temp;
      // if (color1.contrast(color2) > 5) {
      //   return color1;
      // }
      // return color1.darken(5);
    },
    lightenText(color1) {
      let temp = Color(color1);
      let contrast = temp.contrast(color1);

      while (contrast < 15 && temp.hex() !== "#FFFFFF" && temp.hex() !== "#000000") {
        temp = temp.lighten(.2);
        contrast = temp.contrast(color1);
      }
      return temp;
    },
    updateColors() {
      /* eslint-disable no-inner-declarations */
      this.updateColorVariable("--primary-bg-color", this.primaryBgColor);
      //
      this.updateColorVariable("--primary-text-color", this.primaryTextColor);
      this.updateColorVariable("--secondary-bg-color", this.secondaryBgColor);
      this.updateColorVariable("--tertiary-bg-color", this.tertiaryBgColor);
      this.updateColorVariable("--quaternary-bg-color", this.quaternaryBgColor);
    }
  },
  mounted() {
    this.selectedColorScheme = this.colorSchemes.analogous;
    this.updateColors();
  }
};
</script>


<style lang="scss">
  @import './styles/main.scss';
</style>

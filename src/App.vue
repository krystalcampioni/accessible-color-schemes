<template>
  <div id="app">
    <HelloWorld :primaryColor="primary.text"></HelloWorld>

    <div class="controls">
      <input type="color" v-model="mainColor" @change="updateColors" />
      <div class="color-scheme color-scheme--complementary">
        <h2>{{ selectedColorScheme.name }} Colors</h2>
        <ul>
          <li v-for="n in 4" :key="n">some text</li>
        </ul>
      </div>
      <div class="controls__buttons">
        <button :class="{
            '-is-selected': mode === 'dark'
          }"
          @click="selectMode('dark')">dark mode</button>
        <button :class="{
            '-is-selected': mode === 'light'
          }"
          @click="selectMode('light')">light mode</button>

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
const MIN_CONTRAST = 4.5;
import Color from "color";
import contrast from "get-contrast";

import HelloWorld from "./components/HelloWorld";

export default {
  data() {
    return {
      mainColor: "#00fbc8",
      primary: {
        text: "",
        bg: ""
      },
      mode: "dark",
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
    HelloWorld
  },
  computed: {},
  methods: {
    generateTextAndBg(color) {
      let bg = Color(color);
      let text = bg.isLight() ? Color("#000") : Color("#fff");

      // If the contrast is not enough with black or white text,
      // we need to darken/lighten the background color
      if (text.contrast(bg) <= MIN_CONTRAST) {
        bg = text.isLight() ? bg.darken(0.5) : bg.lighten(0.5);
      }

      return {
        text: text.string(),
        bg: bg.string()
      };
    },
    selectColorScheme(scheme) {
      this.selectedColorScheme = scheme;
      this.updateColors();
    },
    selectMode(mode) {
      this.mode = mode;

      this.updateColorVariable(
        "--mode-text-color",
        this.mode === "dark" ? "white" : "black"
      );
      this.updateColorVariable(
        "--mode-bg-color",
        this.mode === "dark" ? "black" : "white"
      );
    },
    updateColorVariable(colorName, newValue) {
      document.documentElement.style.setProperty(colorName, newValue);
    },
    updateColors() {
      this.primary = this.generateTextAndBg(this.mainColor);
      this.secondary = this.generateTextAndBg(
        Color(this.mainColor).rotate(this.selectedColorScheme.angles[0])
      );
      this.tertiary = this.generateTextAndBg(
        Color(this.mainColor).rotate(this.selectedColorScheme.angles[1])
      );
      this.quaternary = this.generateTextAndBg(
        Color(this.mainColor).rotate(this.selectedColorScheme.angles[2])
      );
      this.updateColorVariable("--primary-text-color", this.primary.text);
      this.updateColorVariable("--primary-bg-color", this.primary.bg);

      this.updateColorVariable("--secondary-text-color", this.secondary.text);
      this.updateColorVariable("--secondary-bg-color", this.secondary.bg);

      this.updateColorVariable("--tertiary-text-color", this.tertiary.text);
      this.updateColorVariable("--tertiary-bg-color", this.tertiary.bg);

      this.updateColorVariable("--quaternary-text-color", this.quaternary.text);
      this.updateColorVariable("--quaternary-bg-color", this.quaternary.bg);
    }
  },
  created() {
    this.selectedColorScheme = this.colorSchemes.analogous;
    this.updateColors();
  }
};
</script>


<style lang="scss">
@import "./styles/main.scss";
</style>

<template>
  <div id="app">
    <a target="_blank" href="https://krystalcampioni.github.io/accessible-color-schemes/slides" class="slides-link"> Check the presentation slides
      <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" x="0px" y="0px" viewBox="0 0 100 100" width="15" height="15" class="icon outbound"><path fill="currentColor" d="M18.8,85.1h56l0,0c2.2,0,4-1.8,4-4v-32h-8v28h-48v-48h28v-8h-32l0,0c-2.2,0-4,1.8-4,4v56C14.8,83.3,16.6,85.1,18.8,85.1z"></path> <polygon fill="currentColor" points="45.7,48.7 51.3,54.3 77.2,28.5 77.2,37.2 85.2,37.2 85.2,14.9 62.8,14.9 62.8,22.9 71.5,22.9"></polygon></svg>
    </a>

    <StoreApp :primaryColor="primary.text" />

    <form class="controls">
      <fieldset>
        <legend>Main color:</legend>
        <input type="color" v-model="mainColor" @change="updateColors" />
      </fieldset>

      <div class="controls__buttons">
        <fieldset>
          <legend>Mode:</legend>
          <label for="dark">
            <input
              v-model="mode"
              type="radio"
              name="mode"
              id="dark"
              value="dark"
            />
            dark
          </label>
          <label for="light">
            <input
              v-model="mode"
              type="radio"
              name="mode"
              id="light"
              value="light"
            />
            light
          </label>
        </fieldset>

        <fieldset>
          <legend>Color scheme:</legend>
          <label
            :for="scheme.name"
            v-for="scheme in colorSchemes"
            :key="scheme.name"
          >
            <input
              @click="selectColorScheme(scheme)"
              type="radio"
              :id="scheme.name"
              :value="scheme"
              v-model="selectedColorScheme"
            />
            {{ scheme.name }}
          </label>
        </fieldset>
      </div>

      <fieldset class="color-scheme color-scheme--complementary">
        <legend>Preview:</legend>
        <ul>
          <li v-for="n in 4" :key="n"></li>
        </ul>
      </fieldset>
    </form>
  </div>
</template>

<script>
import Color from "color";
import StoreApp from "./components/StoreApp";

const MIN_CONTRAST = 4.5;

export default {
  data() {
    return {
      mainColor: "#284d7f",

      primary: null,
      secondary: null,
      tertiary: null,
      quaternary: null,

      mode: "light",

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
    StoreApp
  },
  computed: {
    linkColor() {
      let color = Color(this.quaternary.bg);
      let constrast = color.contrast(Color("#fff"));

      // If the contrast is not enough with black or white text,
      // we need to darken/lighten the background color
      while (constrast < MIN_CONTRAST) {
        color = color.darken(0.5);
        constrast = color.contrast(Color("#fff"));
      }
      return color;
    }
  },

  watch: {
    mode() {
      this.updateColorVariable(
        "--mode-text-color",
        this.mode === "dark" ? "#bbbfbd" : "#525458"
      );
      this.updateColorVariable(
        "--mode-bg-color",
        this.mode === "dark" ? "#1e2125" : "#fff"
      );
    }
  },

  methods: {
    updateColorVariable(colorName, newValue) {
      document.documentElement.style.setProperty(colorName, newValue);
    },

    generateTextAndBg(color) {
      let bg = Color(color);
      const text = bg.isLight() ? Color("#000") : Color("#fff");

      let constrast = text.contrast(bg);

      // If the contrast is not enough with black or white text,
      // we need to darken/lighten the background color
      while (constrast < MIN_CONTRAST) {
        bg = text.isLight() ? bg.darken(0.5) : bg.lighten(0.5);
        constrast = text.contrast(bg);
      }
      return { text: text.string(), bg: bg.string() };
    },

    selectColorScheme(scheme) {
      this.selectedColorScheme = scheme;
      this.updateColors();
    },

    updateColors() {
      // Generate colors
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

      // Update CSS variables
      const colorOptions = ["primary", "secondary", "tertiary", "quaternary"];

      colorOptions.forEach(option => {
        this.updateColorVariable(`--${option}-text-color`, this[option].text);
        this.updateColorVariable(`--${option}-bg-color`, this[option].bg);
      });

      this.updateColorVariable("--link-color", this.linkColor);
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

.slides-link {
  background:  #282828;
  color: #ffffff;
  padding: 10px;
  position: fixed;
  text-transform: uppercase;
  bottom: 0;
  left: 0;
}
</style>

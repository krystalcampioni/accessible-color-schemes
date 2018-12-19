<template>
  <div id="app" v-if="selectedColorScheme">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <input type="color" v-model="primaryBgColor" @change="updateColors">
    <div class="color-scheme color-scheme--complementary">

      <h2>{{ selectedColorScheme.name }} Colors</h2>
      <ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </div>

    <button
      v-for="scheme in colorSchemes"
      @click="selectColorScheme(scheme)"
      :key="scheme.name"
    >
      {{ scheme.name }}
    </button>
  </div>
</template>

<script>
import Color from "color";

export default {
  data() {
    return {
      primaryBgColor: "#00fbc8",
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
  computed: {
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
      return color.rotate(this.selectedColorScheme.angles[2]);
    }
  },
  methods: {
    selectColorScheme(scheme) {
      this.selectedColorScheme = scheme;
      this.updateColors();
    },
    updateColorVariable(colorName, newValue) {
      document.documentElement.style.setProperty(colorName, newValue);
    },
    updateColors() {
      this.updateColorVariable("--primary-bg-color", this.primaryBgColor);
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
:root {
  --primary-bg-color: coral;
  --secondary-bg-color: coral;
  --tertiary-bg-color: coral;

  --primary-text-color: white;
}

body {
  background-color: #282828;
  color: var(--primary-text-color);
}

.color-scheme {
  ul {
    list-style: none;
    padding: 0;
  }

  li {
    width: 100%;
    height: 40px;
  }

  li:nth-child(1) {
    background-color: var(--primary-bg-color);
  }
  li:nth-child(2) {
    background-color: var(--secondary-bg-color);
  }
  li:nth-child(3) {
    background-color: var(--tertiary-bg-color);
  }
  li:nth-child(4) {
    background-color: var(--quaternary-bg-color);
  }
}

button {
  background: transparent;
  color: white;
  appearance: none;
  font-size: 1.2rem;
  padding: .5rem 1.2rem;
}
</style>

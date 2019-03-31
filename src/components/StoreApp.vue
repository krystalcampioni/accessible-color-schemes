<template>
  <section>
    <header class="header">
      <div class="flex-container">
        <a href="/" class="header__logo">
          <header-logo :color="primaryColor"></header-logo>
        </a>
        <div class="header__actions">
          <button class="header__button" type="button">
            <span class="-visually-hidden">Search for products</span>
            <icon-cart :color="primaryColor"></icon-cart>
          </button>
          <button class="header__button" type="button">
            <span class="-visually-hidden">Shopping cart</span>
            <icon-search :color="primaryColor"></icon-search>
          </button>
        </div>
      </div>
    </header>

    <nav class="navbar flex-container">
      <ul class="navbar__list">
        <li>home</li>
        <li>winter sports</li>
        <li>street wear</li>
        <li>accessories</li>
        <li>brands</li>
      </ul>
    </nav>

    <section class="banner">
      <div class="banner__content">
        <h1 class="banner__title">Winter won't hurt so bad</h1>
        <div class="banner__nav">
          <button class="banner__nav-button">←</button>
          <button class="banner__nav-button">→</button>
          <h2 class="banner__subtitle">shop snowboarding accessories</h2>
        </div>
      </div>
      <div class="banner__overlay"></div>
    </section>

    <section class="product-highlights flex-container">
      <div class="product-highlights__card" v-for="n in 3" :key="n">
        <img
          :src="`/img/product ${n}.png`"
          alt="black snowboard jacket with fur hood"
        />
        <a href="/snowboard" class="product-highlights__link">
          <span>snowboard</span> view all
        </a>
      </div>
    </section>
  </section>
</template>

<script>
import HeaderLogo from "./HeaderLogo.vue";
import IconCart from "./IconCart.vue";
import IconSearch from "./IconSearch.vue";

export default {
  components: {
    IconCart,
    IconSearch,
    HeaderLogo
  },
  props: {
    primaryColor: {
      type: String,
      required: true
    }
  }
};
</script>

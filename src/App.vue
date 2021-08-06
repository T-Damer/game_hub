<template lang="pug">
v-app(
  :dark='$store.state.dark',
  :class='$store.state.dark ? "grey darken-4" : "grey lighten-4"'
)
  cookie-law(
    theme='blood-orange',
    :buttonText='$t("cookieButton")',
    :message='$t("cookieMessage")'
  )
  //- Navbar
  Snackbar
  v-main
    router-view
</template>

<script lang="ts">
import Vue from 'vue'
import Navbar from '@/components/Navbar.vue'
import Snackbar from '@/components/Snackbar.vue'
import store from '@/store'
import CookieLaw from 'vue-cookie-law'
import { i18n } from '@/plugins/i18n'
import Component from 'vue-class-component'
import { namespace } from 'vuex-class'

const AppStore = namespace('AppStore')
const SnackbarStore = namespace('SnackbarStore')

@Component({ components: { Navbar, CookieLaw, Snackbar } })
export default class App extends Vue {
  @AppStore.State dark!: boolean
  @SnackbarStore.Mutation hideSnackbar!: () => void

  get style() {
    return {
      'background-color': this.dark
        ? 'hsl(154, 10%, 16%)'
        : 'hsl(154, 24%, 94%)',
    }
  }

  created() {
    this.$vuetify.theme.dark = this.dark

    const query = document.querySelector('meta[name="theme-color"]')
    if (query) {
      query.setAttribute(
        'content',
        this.dark ? 'hsl(154, 10%, 16%)' : 'hsl(154, 24%, 94%)'
      )
    }

    this.hideSnackbar()

    document.title = i18n.t('title') as string
  }

  get metaInfo() {
    return {
      title: i18n.t('title') as string,
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@100;400;900&display=swap');

/* VARIABLES */
:root {
  --hue-color: 291; /* Pink - 335, Blue - 240, Green - 154, Orange - 25 */
  --first-color: hsl(var(--hue-color), 53%, 49%);
  --title-color: hsl(var(--hue-color), 53%, 15%);
  --text-color: hsl(var(--hue-color), 12%, 35%);
  --text-color-light: hsl(var(--hue-color), 12%, 65%);
  --white-color: #fff;
  --body-color: hsl(var(--hue-color), 24%, 94%);
}

/*===== Typography =====*/
:root {
  --body-font: 'Lato', sans-serif;
  --big-font-size: 3rem;
  --font-medium: 1.2rem;
  --small-font-size: 0.815rem;
  --smaller-font-size: 0.75rem;
  --tiny-font-size: 0.625rem;
}

@media screen and (min-width: 968px) {
  :root {
    --big-font-size: 3.5rem;
    --font-medium: 1.5rem;
    --small-font-size: 0.875rem;
    --smaller-font-size: 0.815rem;
    --tiny-font-size: 0.75rem;
  }
}

*,
*::before,
*::after {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

/*===== Border-radius =====*/
:root {
  --br-6: 6px;
}

/*===== Margin =====*/
:root {
  --m-0-25: 0.25rem;
  --m-1: 1rem;
  --m-1-5: 1.5rem;
  --m-2-5: 2.5rem;
}

/*=== Scrollbar ===*/
::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-track {
  background: #646464;
}
::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background: var(--white-color);
}
::-webkit-scrollbar-thumb:hover {
  background: #000;
}

body {
  margin: 0;
  font-family: var(--body-font);
  background-color: var(--body-color);
  color: var(--text-color);
}

img {
  height: auto;
}

.slide-fwd-center:hover {
  -webkit-animation: slide-fwd-center 0.45s cubic-bezier(0.25, 0.46, 0.45, 0.94)
    both;
  animation: slide-fwd-center 0.45s cubic-bezier(0.25, 0.46, 0.45, 0.94) both;
}

@-webkit-keyframes slide-fwd-center {
  0% {
    -webkit-transform: translateZ(0);
    transform: translateZ(0);
  }
  100% {
    -webkit-transform: translateZ(160px);
    transform: translateZ(160px);
  }
}
@keyframes slide-fwd-center {
  0% {
    -webkit-transform: translateZ(0);
    transform: translateZ(0);
  }
  100% {
    -webkit-transform: translateZ(160px);
    transform: translateZ(160px);
  }
}

a {
  text-decoration: none;
}
a:hover {
  color: var(--first-color);
}

.special-link:hover {
  background-color: var(--first-color);
  transform: translateX(1s);
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>

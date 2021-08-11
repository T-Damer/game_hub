<template lang="pug">
nav
  v-app-bar(flat, app)
    // Title
    v-toolbar-title
      img.nav__logo(src='img/logo411.png')
    v-spacer
    // Icons
    v-icon(small) twitch
    v-icon(small) account-group
    v-icon(small) controller-classic
    // Dark mode
    v-btn(text, icon, color='grey', @click='toggleMode')
      v-icon(small) brightness-5
    // Language picker
    v-menu(offset-y)
      template(v-slot:activator='{ on }')
        v-btn(text, icon, color='grey', v-on='on') {{ currentLocale.icon }}
      v-list
        v-list-item(
          v-for='locale in locales',
          @click='changeLanguage(locale.code)',
          :key='locale.code'
        )
          v-list-item-title {{ locale.icon }}
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
import { i18n } from '@/plugins/i18n'
import * as api from '@/utils/api'
import { namespace } from 'vuex-class'

const AppStore = namespace('AppStore')

@Component
export default class Navbar extends Vue {
  @AppStore.State dark!: boolean

  @AppStore.Mutation setDark!: (dark: boolean) => void
  @AppStore.Mutation setLanguage!: (language: string) => void

  get locales() {
    return [
      { icon: '🇺🇸', code: 'en' },
      { icon: '🇷🇺', code: 'ru' },
    ]
  }
  get currentLocale() {
    for (const locale of this.locales) {
      if (locale.code === i18n.locale) {
        return locale
      }
    }
  }

  toggleMode() {
    this.setDark(!this.dark)
    ;(this.$vuetify.theme as any).dark = this.dark
  }
  changeLanguage(locale: string) {
    i18n.locale = locale
    this.setLanguage(locale)
    document.title = i18n.t('title') as string // strippedTitle
  }
}
</script>

<style>
:root {
  --nav-height: 4rem;
}

header {
  position: sticky;
  top: 0;
  border-radius: 10px 10px 0px 0px !important;
  margin-top: var(--nav-height);
}

.nav__logo {
  height: 5rem;
}

nav a:link {
  text-decoration: none;
}

nav a:visited {
  text-decoration: none;
}

nav a:hover {
  text-decoration: underline;
}

nav a:active {
  text-decoration: underline;
}
</style>

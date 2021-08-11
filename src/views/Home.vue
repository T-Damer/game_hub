<template lang="pug">
.v-container.pa-4
  // Main content
  v-layout.text-center(column, justify-center, align-center)
    v-flex.pt-4
      .card__container
        Navbar
        h1 {{ $t("Active Games") }}
        .seacrh__box
          span {{ $t("Search") }}
        .game__container
          .game__box
            img(src='img/games/SPIDER-MAN.png')
            .game__info
              span.game__title Spider-Man
              span.game__version PS5 Version
              .game__progress
            h2.game__percents 72%
          .game__box
            img(src='img/games/ACRE.png')
            .game__info
              span.game__title Assassins creed
              span.game__version PS4 Version
              .game__progress
            h2.game__percents 60%
          .game__box
            img(src='img/games/STALKER.png')
            .game__info
              span.game__title S.T.A.L.K.E.R. Call of Pripyat
              span.game__version PC Version
              .game__progress
            h2.game__percents 100%
        .news__container
          h2 Game News
          .news__box
            .news__publisher
              img(src='img/publishers/publisher-logo1.png')
              span.news__title S.T.A.L.K.E.R. 2 Gameplay footage
            .divider__horizontal
            .news__content
              p.news__content An in-engine first-person look at S.T.A.L.K.E.R. 2. Meet the new protagonist called Skif. \thttps://www.xbox.com/games/stalker-2
          .news__box
            .news__publisher
              img(src='img/publishers/publisher-logo1.png')
              span.news__title S.T.A.L.K.E.R. 2 Gameplay footage
            .divider__horizontal
            .news__content
              p.news__content An in-engine first-person look at S.T.A.L.K.E.R. 2. Meet the new protagonist called Skif. \thttps://www.xbox.com/games/stalker-2

      //- v-flex.pt-4
      //-   vue-telegram-login(
      //-     mode='callback',
      //-     telegram-login='mamkintrade_bot',
      //-     @callback='onTelegramAuth',
      //-     radius='3',
      //-     :userpic='false'
      //-   )
      //-   g-signin-button(
      //-     :params='{ client_id: googleClientId }',
      //-     @success='onGoogleSignInSuccess',
      //-     @error='onGoogleSignInError'
      //-   ) {{ $t("home.google") }}
      //-   fb-signin-button(
      //-     :params='{ scope: "email", return_scopes: true }',
      //-     @success='onFacebookSignInSuccess',
      //-     @error='onFacebookSignInError'
      //-   ) {{ $t("home.facebook") }}
      //-   .vk-signin-button(@click.stop='vkDialog = true') {{ $t("home.vk") }}
      //-   .api-signin-button(@click.stop='keyDialog = true') {{ $t("home.key") }}

      //- v-flex.pt-4
      //-   router-link(to='/privacy') {{ $t("home.privacy") }}
</template>

<script lang="ts">
import Vue from 'vue'
import Navbar from '@/components/Navbar.vue'
import axios from 'axios'
import { loginFacebook, loginTelegram, loginGoogle } from '@/utils/api'
import Component from 'vue-class-component'
import { i18n } from '@/plugins/i18n'
import { namespace } from 'vuex-class'
import { User } from '@/models/User'
const { vueTelegramLogin } = require('vue-telegram-login')

const AppStore = namespace('AppStore')
const SnackbarStore = namespace('SnackbarStore')

// FB object is global, declaring here for TS
declare const FB: any
@Component({
  components: {
    vueTelegramLogin,
    Navbar,
  },
})
export default class Home extends Vue {
  @AppStore.Mutation setUser!: (user: User) => void
  @SnackbarStore.Mutation setSnackbarError!: (error: string) => void

  get googleClientId() {
    return '906458427314-vrgseuf6gsroa41l88005jqko24g8shs.apps.googleusercontent.com'
  }

  onFacebookSignInSuccess(response: any) {
    FB.api('/me', async (dude: any) => {
      try {
        const user = await loginFacebook(response.authResponse.accessToken)
        this.setUser(user)
        this.$router.replace('app')
      } catch (err) {
        this.setSnackbarError('errors.facebook')
      }
    })
  }
  onFacebookSignInError(error: Error) {
    this.setSnackbarError('errors.facebook')
  }
  async onGoogleSignInSuccess(googleUser: any) {
    try {
      const user = await loginGoogle(googleUser.getAuthResponse().id_token)
      this.setUser(user)
      this.$router.replace('cabinet')
    } catch (err) {
      this.setSnackbarError('errors.google')
    }
  }
  onGoogleSignInError(error: Error) {
    this.setSnackbarError('errors.google')
  }
  async onTelegramAuth(loginInfo: any) {
    try {
      const user = await loginTelegram(loginInfo)
      this.setUser(user)
      this.$router.replace('cabinet')
    } catch (err) {
      this.setSnackbarError('errors.telegram')
    }
  }
}
</script>

<style>
.fb-signin-button {
  cursor: pointer;
  display: block;
  padding: 10px 46px;
  border-radius: 3px;
  background-color: #647daf;
  color: #fff;
  margin: 10px;
}
.g-signin-button {
  margin: 10px;
  cursor: pointer;
  display: block;
  padding: 10px 46px;
  border-radius: 3px;
  background-color: #ce5658;
  color: #fff;
}
</style>

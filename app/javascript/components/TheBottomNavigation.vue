<template>
  <v-bottom-navigation
    app
    color="green accent-3"
    grow
  >
    <template v-if="authUser">

      <v-btn
        :to="{ name: 'Top' }"
        exact
        v-if="homeActive"
      >
        <span style="font-size:x-small">ホーム</span>

        <v-icon>mdi-home</v-icon>
      </v-btn>

      <v-btn
        :to="{ name: 'Search' }"
        exact
      >
        <span style="font-size:x-small">楽曲検索</span>

        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn
        :to="{ name: 'MyLibrary' }"
        exact
      >
        <span style="font-size:x-small">マイライブラリ</span>

        <v-icon>mdi-music-box-multiple</v-icon>
      </v-btn>

      <v-btn
        :to="{ name: 'UserInformation' }"
        exact
      >
        <span style="font-size:x-small">ユーザー情報</span>

        <v-icon>mdi-account</v-icon>
      </v-btn>

      <v-btn
        v-if="logoutActive"
        @click="handleLogout"
      >
        <span style="font-size:x-small">ログアウト</span>

        <v-icon>mdi-logout</v-icon>
      </v-btn>
    </template>

    <template v-else>
      <v-btn
        :to="{ name: 'Top' }"
        exact
      >
        <span style="font-size:x-small">ホーム</span>

        <v-icon>mdi-home</v-icon>
      </v-btn>

      <v-btn
        :to="{ name: 'Register' }"
        exact
      >
        <span style="font-size:x-small">ユーザー登録</span>

        <v-icon>mdi-account-plus</v-icon>
      </v-btn>

      <v-btn
        :to="{ name: 'Login' }"
        exact
      >
        <span style="font-size:x-small">ログイン</span>

        <v-icon>mdi-login</v-icon>
      </v-btn>
    </template>
  </v-bottom-navigation>
</template>

<script>
import { mapGetters, mapActions } from "vuex"

export default {
  name: "TheBottomNavigation",

  data() {
    return {
      homeActive: false,
      logoutActive: false
    }
  },

  created() {
    window.addEventListener('resize', this.handleShowLogout)
    this.handleShowLogout()
  },

  computed: {
    ...mapGetters("users", ["authUser"])
  },

  methods: {
    ...mapActions("users", ["logoutUser"]),

    async handleLogout() {
      try {
        await this.logoutUser()
        this.$router.push({ name: "Top" }).catch(err => {})
        this.$store.dispatch("flashMessages/showMessage",
          {
            message: "ログアウトしました",
            type: "success",
            status: true
          }
        )
      } catch(error) {
        console.log(error)
        this.$store.dispatch("flashMessages/showMessage",
          {
            message: "ログアウトに失敗しました",
            type: "error",
            status: true
          }
        )
      }
    },

    handleShowLogout() {
      if (window.innerWidth > 330) {
        this.homeActive = true
        this.logoutActive = true
      } else if (window.innerWidth > 250) {
        this.homeActive = true
        this.logoutActive = false
      } else {
        this.homeActive = false
        this.logoutActive = false
      }
    },
  }
}
</script>

<style scoped>
</style>
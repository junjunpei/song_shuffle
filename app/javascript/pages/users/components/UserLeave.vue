<template>
  <v-row
    justify="center"
    class="mt-5"
  >
    <v-btn
      color="error"
      text
      @click.stop="handleOpenLeaveModal"
    >
      退会する
    </v-btn>
    <v-dialog
      max-width="500"
      id="user-leave-modal"
      v-model="dialog"
    >
      <v-card>
        <v-card-title class="text-h5">
          退会する
        </v-card-title>

        <v-divider />

        <v-card-text class="red--text mt-6">
          退会される場合は以下の注意事項をよくお読みいただき、
          同意するにチェックを入れた上で退会ボタンを押してください。
        </v-card-text>
        <v-card-text>
          ・一度退会をすると、
          ユーザー情報やマイライブラリに追加した楽曲は全て消去されます。
        </v-card-text>
        <v-card-text>
          ・再度ご利用いただく際は、会員登録が必要です。
          また、楽曲も再度追加し直す必要がございます。
        </v-card-text>

        <v-checkbox
          v-model="checkbox"
          class="ml-5"
          color="info"
          id="check-box"
        >
          <template v-slot:label>
            <div class="light-blue--text text--accent-3">
              上記の注意事項に同意する。
            </div>
          </template>
        </v-checkbox>

        <v-card-actions>

          <v-spacer />

          <v-btn
            color="green accent-3"
            text
            @click="handleCloseLeaveModal"
          >
            キャンセル
          </v-btn>

          <v-btn
            color="error"
            text
            type="submit"
            :disabled="!checkbox || loading"
            @click="handleLeaveUser"
          >
            退会
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  name: 'UserLeave',

  props: {
    loading: {
      type: Boolean,
      required: true
    }
  },

  data() {
    return {
      checkbox: false,
      dialog: false
    }
  },

  watch: {
    dialog: function(boolean) {
      if (boolean == false) {
        this.checkbox = false
        this.handleCloseLeaveModal()
      }
    }
  },

  methods: {
    handleOpenLeaveModal() {
      this.dialog = true
    },

    handleCloseLeaveModal() {
      this.dialog = false
    },

    handleLeaveUser() {
      this.$emit("leave-user")
    },

    submit() {
      this.$refs.observer.validate()
    }
  }
}
</script>
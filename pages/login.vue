<template>
  <v-container class="fill-height bg" fluid>
    <v-row justify="center" align="end" class="mx-0 fill-height">
      <v-col cols="12" md="5" class="pt-5 px-0">
        <v-card-title>
          <v-spacer></v-spacer>
          <img
            src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/Logo-SCB_Hitam.png"
            width="300px"
          />
          <v-spacer></v-spacer>
        </v-card-title>
        <v-card-text>
          <v-form @submit="userLogin">
            <v-row>
              <v-col cols="12" class="text-center pb-7">
                <div class="headline">Sistem Pembayaran CEC</div>
                <div class="subtitle-1">Silahkan masuk dengan akun anda</div>
              </v-col>
              <v-col cols="12" class="py-2">
                <v-text-field
                  label="Nama pengguna"
                  color="green"
                  solo
                  v-model="login.email"
                  hide-details
                  prepend-inner-icon="mdi-account"
                ></v-text-field>
              </v-col>
              <v-col cols="12" class="py-2">
                <v-text-field
                  label="Kata sandi"
                  color="green"
                  solo
                  v-model="login.password"
                  hide-details
                  type="password"
                  prepend-inner-icon="mdi-key"
                ></v-text-field>
              </v-col>
              <v-col cols="12" class="pt-5">
                <v-btn
                  block
                  color="green"
                  dark
                  height="40px"
                  @click.stop="userLogin"
                  >Masuk</v-btn
                >
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
      </v-col>
      <v-col cols="12" class="py-0">
        <div class="text-center">
          <div class="caption">Sistem hasil kerja sama</div>
          <img
            src="https://cs.ipb.ac.id/wp-content/uploads/2019/06/logoipbilkomdarkhorizontal.png"
            width="250px"
          />
          <div class="overline">Rian Ardiana Prapanca G64170085</div>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  layout: 'blank',
  auth: 'guest',
  data() {
    return {
      login: {
        email: '',
        password: ''
      }
    }
  },
  methods: {
    async userLogin() {
      try {
        let response = await this.$auth.loginWith('local', { data: this.login })
        console.log(response)
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style scoped>
.bg {
  background: linear-gradient(
      0deg,
      rgba(252, 252, 252, 0.4),
      rgba(0, 255, 0, 0.1)
    ),
    url(~assets/img/bg.svg) no-repeat center center;
  background-size: contain;
}
body {
  overflow: hidden;
}
</style>

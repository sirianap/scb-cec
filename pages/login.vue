<template>
  <v-container class="fill-height bg" fluid>
    <v-row justify="center" align="end" class="mx-0 fill-height">
      <v-col cols="12" md="4" class="pt-5 px-0">
        <v-card class="pa-6 login-card">
          <v-card-title>
            <v-spacer></v-spacer>
            <img src="~assets/img/Logo-SCB-Hitam.png" width="250px" />
            <v-spacer></v-spacer>
          </v-card-title>
          <v-card-text>
            <v-form @submit="userLogin">
              <v-row>
                <v-col cols="12" class="text-center pb-7">
                  <div class="headline primary--text">Sistem Pembayaran CEC</div>
                  <div class="subtitle-1 black--text">Silahkan masuk dengan akun anda</div>
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
                  <v-btn block color="green" dark height="40px" @click.stop="userLogin">Masuk</v-btn>
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
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  head: {
    title: 'Login'
  },
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
  background: linear-gradient(0deg, rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.6)),
    url(~assets/img/bg-login.jpeg) no-repeat top center;
  background-size: cover;
}
body {
  overflow: hidden;
}
.login-card {
  background: rgba(255, 255, 255, 0.75);
}
</style>

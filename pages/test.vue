<template>
  <v-layout row justify-center align-end fill-height>
    <v-flex md4 xs12 lg4 sm11>
      <v-card class="py-2 my-5" elevation="2">
        <v-card-text class="text-center pa-5">
          <img
            src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/Logo-SCB_Hitam.png"
            width="200px"
          />
          <div class="title">Masuk</div>
          <div class="subtitle-1">Masuk dengan akun anda</div>
          <v-text-field
            label="Email"
            outlined
            color="green"
            hide-details
            class="py-2"
            prepend-inner-icon="mdi-account"
          ></v-text-field>
          <v-text-field
            label="Kata sandi"
            outlined
            color="green"
            hide-details
            class="py-2"
            type="password"
            prepend-inner-icon="mdi-key"
          ></v-text-field>
          <v-btn block color="green" height="50px" class="mt-5" dark to="/">Masuk</v-btn>
        </v-card-text>
      </v-card>
      <div class="text-center py-5">
        <div class="caption">Sistem hasil kerja sama</div>
        <img
          src="https://cs.ipb.ac.id/wp-content/uploads/2019/06/logoipbilkomdarkhorizontal.png"
          width="250px"
        />
        <!-- <div class="subtitle-2">Rian Ardiana Prapanca G64170085</div> -->
      </div>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  layout: 'blank'
}
</script>

<style >
body {
  overflow: hidden;
}
</style>
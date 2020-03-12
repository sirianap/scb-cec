<template>
  <v-layout row justify-center align-center>
    <v-flex md12 xs12 sm12 lg12>
      <v-card height="400px">
        <v-card-text class="pb-0">
          <video
            id="player"
            ref="player"
            autoplay
            width="100%"
            height="350px"
          ></video>
        </v-card-text>
      </v-card>
    </v-flex>
    <v-flex md12 xs12 sm12 lg12>
      <v-card>
        <v-card-title>
          Pembayaran
          <v-spacer></v-spacer>
          <v-btn dark color="green">Scan Barcode</v-btn>
        </v-card-title>
        <v-card-text>
          <v-text-field
            label="NISN Siswa"
            color="green"
            outlined
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn block color="green" dark>Bayar</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import { MEDIA } from '../mixins/Media'
export default {
  data() {
    return {
      toggle: true
    }
  },
  mixins: [MEDIA],
  mounted() {
    this.initializeMedia(this.$refs.player)
  },
  watch: {}
}
</script>

<style></style>

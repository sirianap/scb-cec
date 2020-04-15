<template>
  <v-layout row justify-center align-center fill-height>
    <v-flex md12 xs12 sm12 lg12>
      <v-card>
        <v-card-text class="pb-0">
          <video id="player" ref="player" preload autoplay width="100%" height="350px"></video>
        </v-card-text>
      </v-card>
    </v-flex>
    <v-flex md12 xs12 sm12 lg12>
      <v-card class="pembayaran">
        <v-card-title>
          Pembayaran
          <v-spacer></v-spacer>
          <v-btn color="green" disabled>Scan Barcode</v-btn>
        </v-card-title>
        <v-card-text>
          <v-text-field label="NISN Siswa" color="green" outlined hide-details></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn block color="green" dark @click.stop="dialog_pembayaran = true">Bayar</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
    <v-dialog v-model="dialog_pembayaran" persistent width="600px">
      <v-card>
        <v-card-title>Pembayaran</v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="12" class="pa-0">
              <v-list>
                <v-list-item>
                  <v-list-item-avatar>
                    <img
                      src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
                    />
                  </v-list-item-avatar>
                  <v-list-item-content>
                    <v-list-item-title>Rian Ardiana Prapanca</v-list-item-title>
                    <v-list-item-subtitle>G64170085</v-list-item-subtitle>
                    <v-list-item-subtitle>Saldo : Rp. 20.000</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-col>
            <v-col cols="12">
              <v-text-field
                color="green"
                label="Nominal"
                type="number"
                outlined
                prepend-inner-icon="mdi-cash"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click.stop="dialog_pembayaran = false">Batal</v-btn>
          <v-btn dark color="green">Bayar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialog_konfirmasi" width="600px" persistent>
      <v-card>
        <v-card-title>Konfirmasi pembayaran</v-card-title>
        <v-card-text>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Vitae distinctio obcaecati laboriosam, architecto hic pariatur quibusdam maiores veritatis facilis repellat iste neque recusandae magnam quo sunt! Earum commodi beatae recusandae.</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click.stop="dialog_konfirmasi = false">Batal</v-btn>
          <v-btn color="green" dark>Konfirmasi</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
import { MEDIA } from '../mixins/Media'
export default {
  data() {
    return {
      dialog_pembayaran: false,
      dialog_konfirmasi: false
    }
  },
  mixins: [MEDIA],
  mounted() {
    this.initializeMedia(this.$refs.player)
  },
  watch: {}
}
</script>

<style scoped>
.pembayaran {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}
</style>

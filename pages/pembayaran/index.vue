<template>
  <v-layout row justify-center align-center fill-height class="bg-pembayaran">
    <v-flex md12 xs12 sm12 lg12 style="height:60vh">
      <v-row align="center" class="pa-0 ma-0 fill-height">
        <v-card-text class="pb-0">
          <v-img
            src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
            height="130px"
            contain
          ></v-img>
          <v-list-item class="text-center">
            <v-list-item-content>
              <v-list-item-title>{{detail.nama}}</v-list-item-title>
              <v-list-item-subtitle>{{detail.nis}}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
          <v-list-item v-if="detail.saldo_total!=null">
            <v-list-item-content>
              <v-list-item-title>Saldo DigitCard</v-list-item-title>
              <v-list-item-title>Saldo Total</v-list-item-title>
            </v-list-item-content>
            <v-list-item-content class="text-right">
              <v-list-item-title>Rp. {{detail.saldo_digitcard}}</v-list-item-title>
              <v-list-item-title>Rp. {{detail.saldo_total}}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-card-text>
      </v-row>
    </v-flex>
    <v-flex md12 xs12 sm12 lg12 class="pembayaran" style="height:30vh">
      <v-card class="ma-0 pembayaran">
        <v-card-title>
          Pembayaran
          <v-spacer></v-spacer>
          <!-- <v-btn color="green" disabled>Scan Barcode</v-btn> -->
        </v-card-title>
        <v-card-text>
          <v-text-field
            label="Nomor Induk Siswa"
            color="green"
            outlined
            counter
            autofocus
            hide-details
            v-model="nis"
            type="number"
            v-on:keyup.enter="lihatDetail(nis)"
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn block color="green" dark @click.stop="goDetail">Bayar</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>

    <v-dialog v-model="dialog_pembayaran" persistent width="600px">
      <v-card>
        <v-card-title>
          Pembayaran
          <v-spacer></v-spacer>
          <v-btn fab icon small @click.stop="dialog_pembayaran = false">
            <v-icon color="red">mdi-close</v-icon>
          </v-btn>
        </v-card-title>
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
                    <v-list-item-title>{{ detail.nama }}</v-list-item-title>
                    <v-list-item-subtitle>
                      {{
                      detail.nis
                      }}
                    </v-list-item-subtitle>
                    <v-list-item-subtitle>
                      Saldo : Rp.
                      {{ detail.saldo_digitcard }}
                    </v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-col>
            <v-col cols="12">
              <v-text-field
                color="green"
                autofocus
                label="Nominal"
                type="number"
                outlined
                v-model="input.bayar.nominal"
                prepend-inner-icon="mdi-cash"
                v-on:keyup.enter="bayar()"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <!-- <v-spacer></v-spacer> -->

          <v-btn dark block color="green" @click.stop="bayar">Bayar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialog_konfirmasi" width="600px" persistent>
      <v-card>
        <v-card-title>Konfirmasi pembayaran</v-card-title>
        <v-card-text>
          Lorem, ipsum dolor sit amet consectetur adipisicing elit. Vitae
          distinctio obcaecati laboriosam, architecto hic pariatur quibusdam
          maiores veritatis facilis repellat iste neque recusandae magnam quo
          sunt! Earum commodi beatae recusandae.
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click.stop="dialog_konfirmasi = false">Batal</v-btn>
          <v-btn color="green" dark>Konfirmasi</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- DIALOG ERROR -->
    <v-snackbar v-model="error.status" color="red" bottom left>
      {{error.msg}}
      <v-btn dark icon small @click="error.status = false" fab>
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </v-snackbar>
    <!-- DIALOG SUKSES BAYAR -->
    <v-snackbar v-model="sukses_bayar" color="primary" multi-line>
      Transaksi berhasil
      <v-btn dark icon small @click="sukses_bayar = false" fab>
        <v-icon>mdi-close-thick</v-icon>
      </v-btn>
    </v-snackbar>
  </v-layout>
</template>

<script>
export default {
  data() {
    return {
      dialog_pembayaran: false,
      dialog_konfirmasi: false,
      nis: null,
      count: null,
      detail: {
        nama: null,
        nis: null,
        saldo_digitcard: null,
        saldo_total: null
      },
      error: {
        status: false,
        msg: null
      },
      sukses_bayar: false,
      input: {
        bayar: {
          siswa_id: null,
          nominal: null,
          mutasi_type: 'out',
          saldo_type: 'card'
        }
      }
    }
  },
  methods: {
    async lihatDetail(nis) {
      try {
        const siswa = await this.$axios.$get('siswa/' + nis)
        this.detail = siswa
        this.input.bayar.siswa_id = this.detail.id
        // this.dialog_pembayaran = true
      } catch (e) {
        console.error(e)
        this.error.status = true
        this.error.msg = 'Siswa tidak ditemukan'
      }
    },
    goDetail() {
      if (this.detail.nis) {
        this.$router.push({ path: `/pembayaran/${this.detail.nis}` })
      } else {
        this.error.status = true
        this.error.msg = 'Harap masukan Nomor Induk Siswa'
      }
    },
    async bayar() {
      try {
        const res = await this.$axios({
          method: 'post',
          url: '/transaksi',
          data: this.input.bayar
        })
        this.dialog_pembayaran = false
        this.sukses_bayar = true
        console.log(res)
      } catch (e) {
        console.log(e)
        this.error.status = true
        this.error.msg = e
      }
    }
  },
  mounted() {},
  watch: {
    nis: function(val) {
      if (val.length >= 8) this.lihatDetail(val)
    }
  }
}
</script>

<style scoped>
.pembayaran {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}
.bg-pembayaran {
  background: linear-gradient(
      rgba(255, 255, 255, 0.4),
      rgba(255, 255, 255, 0.9)
    ),
    url('~assets/img/bg-payment1.svg') no-repeat;
  background-size: contain;
  background-position: center center;
}
</style>

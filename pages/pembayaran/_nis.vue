<template>
  <div class="bg-payment" style="height:70vh">
    <v-layout column>
      <v-flex>
        <v-row align="center" class="pb-3 px-2">
          <v-btn icon @click.stop="$router.go(-1)">
            <v-icon>mdi-arrow-left</v-icon>
          </v-btn>
          <span class="title">Pembayaran</span>
        </v-row>
      </v-flex>
      <v-flex>
        <v-card class="transparent">
          <v-list-item>
            <v-list-item-avatar>
              <img
                src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
              />
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title>{{ siswa.nama }}</v-list-item-title>
              <v-list-item-subtitle>{{ siswa.nis }}</v-list-item-subtitle>
              <v-list-item-subtitle
                >Rp {{ siswa.saldo_digitcard }}</v-list-item-subtitle
              >
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-flex>
      <!-- <v-flex>
        <v-card class="my-5">
          <v-card-title>Produk</v-card-title>
          <v-row class="py-0 px-5">
            <v-col class="pa-0">
              <v-text-field
                dense
                autofocus
                type="number"
                label="Masukan nomor produk"
                outlined
                append-icon="mdi-plus"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-data-table hide-default-footer :headers="header_produk">
            <template v-slot:footer>
              <v-divider></v-divider>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Total transaksi</v-list-item-title>
                </v-list-item-content>
                <v-list-item-content class="text-right">
                  <v-list-item-title>Rp. 0</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
          </v-data-table>
        </v-card>
      </v-flex>-->
      <v-flex>
        <v-text-field
          autofocus
          type="number"
          label="Masukan nomor produk"
          solo
          v-model="nomor_produk"
          hide-details
          append-icon="mdi-plus"
          class="py-4"
          v-on:keyup.enter="getProduk(nomor_produk)"
        ></v-text-field>
        <v-divider></v-divider>

        <v-list class="transparent" style="min-height:40vh">
          <template v-if="detail.length == 0">
            <v-row justify="center" class="fill-height" align="center">
              <v-col class="text-center">
                <div class="headline">Belum ada produk</div>
                <div class="subtitle">Silahkan masukan nomor produk</div>
              </v-col>
            </v-row>
          </template>
          <template v-for="item in detail">
            <div :key="item.id">
              <v-list-item two-line>
                <v-list-item-avatar class="my-auto">
                  <v-text-field
                    v-model="item.jumlah_beli"
                    class="text-center headline"
                    flat
                  ></v-text-field>
                  <!-- <span class="headline">{{item.jumlah_beli}}</span> -->
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title>{{ item.nama }}</v-list-item-title>
                  <v-list-item-subtitle>{{ item.nomor }}</v-list-item-subtitle>
                  <v-list-item-subtitle></v-list-item-subtitle>
                </v-list-item-content>
                <v-list-item-content class="text-right">
                  <v-list-item-title>Rp {{ harga(item) }}</v-list-item-title>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn icon color="red" @click.stop="deleteItem(item)">
                    <v-icon>mdi-close</v-icon>
                  </v-btn>
                </v-list-item-action>
              </v-list-item>
              <v-divider></v-divider>
            </div>
          </template>
        </v-list>
      </v-flex>
      <v-flex id="btn-bayar" class="transparent">
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>Jumlah</v-list-item-title>
          </v-list-item-content>
          <v-list-item-content class="text-right">
            <v-list-item-title class="headline"
              >Rp. {{ hargaTotal(detail) }}</v-list-item-title
            >
          </v-list-item-content>
        </v-list-item>
        <v-btn block color="primary" @click.stop="bayar()">Bayar</v-btn>
      </v-flex>
    </v-layout>
    <!-- SNACK BAR -->
    <!-- DIALOG SUKSES -->
    <v-snackbar v-model="sukses.status" color="primary">
      {{ sukses.msg }}
      <v-btn dark icon small @click="sukses.status = false" fab>
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </v-snackbar>

    <!-- DIALOG ERROR -->
    <v-snackbar v-model="error.status" color="red">
      {{ error.msg }}
      <v-btn dark icon small @click="error.status = false" fab>
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </v-snackbar>
  </div>
</template>

<script>
export default {
  data() {
    return {
      header_produk: [
        { text: 'Nomor Produk', value: 'nomor_produk' },
        { text: 'Nama Produk', value: 'nama' },
        { text: 'Jumlah', value: 'jumlah' },
        { text: 'Harga Satuan', value: 'harga_satuan' },
        { text: 'Harga Total', value: 'harga_total' }
      ],
      nomor_produk: '',
      siswa: {
        id: null,
        nama: null,
        nis: null,
        saldo_digitcard: null,
        saldo_total: null
      },
      sukses: {
        status: false,
        msg: null
      },
      error: {
        status: false,
        msg: null
      },
      detail: []
    }
  },
  methods: {
    async getSiswa(nis) {
      try {
        const res = await this.$axios.$get('siswa/' + nis)
        this.siswa = res
      } catch (e) {
        console.error(e)
      }
    },
    async getProduk(nomor) {
      try {
        const res = await this.$axios.$get('produk/' + nomor)
        const found = this.detail.find(el => el.id == res.id)
        if (found) {
          found.jumlah_beli += 1
        } else {
          res.jumlah_beli = 1
          this.detail.push(res)
          console.log(res)
        }
        this.nomor_produk = ''
      } catch (e) {
        console.error(e)
      }
    },
    harga(obj) {
      return obj.harga_jual * obj.jumlah_beli
    },
    hargaTotal(detail) {
      let sum = 0
      detail.forEach(el => {
        sum += el.harga_jual * el.jumlah_beli
      })
      return sum
    },
    deleteItem(obj) {
      var removeIndex = this.detail
        .map(function(item) {
          return item.id
        })
        .indexOf(obj.id)
      this.detail.splice(removeIndex, 1)
    },
    async bayar() {
      if (this.detail.length != 0) {
        try {
          let data = this.siswa
          data.detail = this.detail
          const res = await this.$axios({
            method: 'post',
            url: '/transaksi/detail',
            data: data
          })
          console.log(res)
          this.sukses.status = true
          this.sukses.msg = 'Pembayaran berhasil'
          setTimeout(() => {
            this.$router.push('/pembayaran')
          }, 2000)
        } catch (e) {
          console.error(e)
          this.error.status = true
          this.error.msg =
            'Saldo tidak mencukupi, silahkan hapus beberapa produk'
        }
      } else {
        this.error.status = true
        this.error.msg = 'Silahkan masukan produk terlebih dahulu'
      }
    }
  },
  watch: {
    nomor_produk: function(val) {
      if (val.length >= 4) this.getProduk(val)
    }
  },
  computed: {},
  mounted() {
    this.getSiswa(this.$route.params.nis)
    console.log(this.$route.params.nis)
  }
}
</script>

<style scoped>
#btn-bayar {
  position: relative;
  margin: 5px;
  right: 0;
  left: 0;
  bottom: 0;
}
.bg-payment {
  background: url('~assets/img/bg-payment.svg') no-repeat;
  background-size: contain;
  background-position: bottom center;
}
.transparent {
  background: rgba(255, 255, 255, 0.8) !important;
}</style
>>

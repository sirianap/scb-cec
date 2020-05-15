<template>
  <div class="bg-dashboard">
    <v-layout column>
      <v-flex>
        <v-row>
          <v-col cols="12" md="3" class="pt-0">
            <v-menu
              v-model="menu1"
              :close-on-content-click="false"
              max-width="290"
              offset-y
            >
              <template v-slot:activator="{ on }">
                <v-text-field
                  :value="dateRangeText"
                  clearable
                  prepend-icon="mdi-calendar"
                  label="Tanggal"
                  readonly
                  hide-details
                  v-on="on"
                  @click:clear="
                    dates = []
                    getTransaksi()
                  "
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="dates"
                @change="
                  menu1 = false
                  getTransaksi()
                "
                range
                no-title
              ></v-date-picker>
            </v-menu>
          </v-col>
        </v-row>
      </v-flex>
      <v-flex md12 xs12 lg12 sm12>
        <v-row>
          <v-col cols="12" md="4">
            <v-card>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title class="primary--text"
                    >Pembayaran masuk</v-list-item-title
                  >
                  <v-list-item-subtitle class="display-1"
                    >Rp
                    {{
                      pembayaranMasuk(daftar_transaksi)
                    }}</v-list-item-subtitle
                  >
                </v-list-item-content>
                <v-list-item-icon>
                  <v-icon size="50px" color="primary">mdi-cash-multiple</v-icon>
                </v-list-item-icon>
              </v-list-item>
            </v-card>
          </v-col>
          <v-col cols="12" md="4">
            <v-card>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title class="primary--text"
                    >Keuntungan</v-list-item-title
                  >
                  <v-list-item-subtitle class="display-1"
                    >Rp On Progress</v-list-item-subtitle
                  >
                </v-list-item-content>
                <v-list-item-icon>
                  <v-icon size="50px" color="primary"
                    >mdi-flower-tulip-outline</v-icon
                  >
                </v-list-item-icon>
              </v-list-item>
            </v-card>
          </v-col>
          <v-col cols="12" md="4">
            <v-card>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title class="primary--text"
                    >Jumlah Transaksi</v-list-item-title
                  >
                  <v-list-item-subtitle class="display-1">
                    {{ daftar_transaksi.length }}
                  </v-list-item-subtitle>
                </v-list-item-content>
                <v-list-item-icon>
                  <v-icon size="50px" color="primary">mdi-transition</v-icon>
                </v-list-item-icon>
              </v-list-item>
            </v-card>
          </v-col>
        </v-row>
      </v-flex>
      <v-flex md12 xs12 lg12 sm12>
        <v-row>
          <v-col cols="12">
            <v-card>
              <v-card-title>Riwayat pembayaran</v-card-title>
              <!-- <pre>{{daftar_transaksi}}</pre> -->
              <v-data-table :headers="header" :items="daftar_transaksi">
                <template v-slot:item.created_at="{ item }">{{
                  $moment(item.created_at).format('Do MMM YYYY, h:mm a')
                }}</template>
                <template v-slot:item.id="{ item }">
                  <v-btn
                    text
                    small
                    color="primary"
                    @click.stop="detailTransaksi(item.id)"
                    >Lihat detail</v-btn
                  >
                </template>
              </v-data-table>
            </v-card>
          </v-col>
        </v-row>
      </v-flex>
    </v-layout>
    <v-dialog v-model="dialog_transaksi" width="400px" scrollable persistent>
      <v-card class="bg-card" id="print">
        <v-card-title>
          Transaksi
          <v-spacer></v-spacer>
          <v-btn icon @click.stop="dialog_transaksi = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <div>
            {{
              $moment(this.transaksi.created_at).format('hh:mm a, Do MMMM Y')
            }}
          </div>
          <div>
            <span class="primary--text">Nomor transaksi</span>
            <span class="font-weight-medium float-right">{{
              this.transaksi.id
            }}</span>
          </div>
          <div>
            <span class="primary--text">Pembeli</span>
            <span class="font-weight-medium float-right">{{
              this.transaksi.siswa.nama
            }}</span>
          </div>
          <div>
            <span class="primary--text">Kasir</span>
            <span class="font-weight-medium float-right">{{
              this.transaksi.created_by.name
            }}</span>
          </div>
          <div>
            <span class="primary--text">Nominal</span>
            <span class="font-weight-medium float-right"
              >Rp {{ this.transaksi.nominal }}</span
            >
          </div>
          <!-- <div>
            <v-btn small text color="info" block @click.stop="downloadWithCSS"
              >Simpan kedalam pdf</v-btn
            >
          </div> -->
          <v-list dense class="transparent">
            <template v-for="item in transaksi.detail">
              <v-list-item class="px-0" :key="item.id">
                <v-list-item-avatar>
                  <div class="headline primary--text">{{ item.jumlah }}</div>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title>{{ item.produk.nama }}</v-list-item-title>
                  <v-list-item-subtitle>{{
                    item.produk.nomor
                  }}</v-list-item-subtitle>
                </v-list-item-content>
                <v-list-item-content class="text-right">
                  <v-list-item-title
                    >Rp {{ item.harga_total }}</v-list-item-title
                  >
                </v-list-item-content>
              </v-list-item>
            </template>
          </v-list>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import jsPDF from 'jspdf'
import domtoimage from 'dom-to-image'
var node = document.getElementById('print')
export default {
  data() {
    return {
      dates: [new Date().toISOString().substr(0, 10)],
      menu1: false,
      header: [
        { text: 'Waktu', value: 'created_at' },
        { text: 'Pembeli', value: 'siswa.nama' },
        { text: 'Nominal', value: 'nominal' },
        { text: 'PIC', value: 'created_by.name' },
        { text: '', value: 'id' }
      ],
      daftar_transaksi: [],
      transaksi: {
        siswa: {
          nama: ''
        },
        created_by: {
          name: ''
        }
      },

      dialog_transaksi: false
    }
  },
  methods: {
    async getTransaksi() {
      try {
        const res = await this.$axios.get('/transaksi', {
          params: {
            from: this.dates[0],
            to: this.dates[1]
          }
        })
        this.daftar_transaksi = res.data
        console.log(res)
      } catch (e) {
        console.log(e)
      }
    },
    async detailTransaksi(id) {
      try {
        const res = await this.$axios.get('/transaksi/' + id)
        this.transaksi = res.data
        this.dialog_transaksi = true
        console.log(res)
      } catch (e) {
        console.log(e)
      }
    },
    pembayaranMasuk(detail) {
      let sum = 0
      detail.forEach(el => {
        sum += el.nominal
      })
      return sum
    },
    downloadWithCSS() {
      /** WITH CSS */
      domtoimage
        .toPng(document.getElementById('print'))
        .then(function(dataUrl) {
          var img = new Image()
          img.src = dataUrl
          const doc = new jsPDF({
            orientation: 'portrait',
            // unit: "pt",
            format: [400, 500]
          })
          doc.addImage(img, 'JPEG', 20, 20)
          const date = new Date()
          const filename = 'INV/1.pdf'
          doc.save(filename)
        })
        .catch(function(error) {
          console.error('oops, something went wrong!', error)
        })
    }
  },
  computed: {
    dateRangeText() {
      return this.dates.join(' ~ ')
    }
  },
  mounted() {
    this.getTransaksi()
  }
}
</script>
<style scoped>
.bg-dashboard {
  background: linear-gradient(
      rgba(255, 255, 255, 0.7),
      rgba(255, 255, 255, 0.7)
    ),
    url('~assets/img/bg-dashboard.svg') no-repeat;
  background-position: center right;
  background-attachment: fixed;
  background-size: contain;
  min-height: 85vh;
}
.bg-card {
  background: linear-gradient(
      rgba(255, 255, 255, 0.97),
      rgba(255, 255, 255, 0.97)
    ),
    url('https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png');
  background-color: white;
  background-position: center center;
  background-attachment: fixed;
  background-size: 50%;
}
</style>

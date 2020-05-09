<template>
  <div class="bg-siswa">
    <!-- MAIN CONTENT -->
    <v-layout column="">
      <v-flex>
        <v-text-field
          outlined
          label="Cari siswa"
          hide-details
          v-model="search"
          prepend-inner-icon="mdi-account-search"
          color="green"
        ></v-text-field>
      </v-flex>
      <v-flex>
        <v-list class="transparent">
          <template v-for="siswa in filteredSiswa">
            <div :key="siswa.id">
              <v-list-item
                @click.stop="
                  lihatDetail(siswa)
                  drawer = true
                "
              >
                <v-list-item-avatar>
                  <img
                    src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
                    width="100%"
                  />
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title>
                    {{ siswa.nama }}
                  </v-list-item-title>
                  <v-list-item-subtitle>{{ siswa.nis }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-divider></v-divider>
            </div>
          </template>
        </v-list>
      </v-flex>
      <!-- <v-flex>
        <v-card>
          <v-card-title>
            Siswa
            <v-spacer></v-spacer>
            <v-btn color="green" dark class="px-5" @click.stop="dialog_tambah_siswa = true">Tambah</v-btn>
          </v-card-title>
          <v-card-text>
            <v-text-field
              outlined
              label="Cari siswa"
              hide-details
              v-model="search"
              prepend-inner-icon="mdi-account-search"
              color="green"
            ></v-text-field>
            <v-data-table :headers="table_siswa_header" :items="daftar_siswa" :search="search">
              <template v-slot:item.id="{ item }">
                <v-btn
                  text
                  small
                  color="primary"
                  @click.stop="
                    lihatDetail(item)
                    drawer = true
                  "
                >Lihat detail</v-btn>
              </template>
            </v-data-table>
          </v-card-text>
        </v-card>
      </v-flex> -->
    </v-layout>
    <div class="add-btn">
      <v-btn
        fab
        color="primary"
        class="ma-4"
        @click.stop="dialog_tambah_siswa = true"
      >
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </div>

    <!-- DIALOG TOP UP -->
    <v-dialog v-model="dialog_topup" persistent width="600px">
      <v-card>
        <v-card-title>
          Top Up
          <v-spacer></v-spacer>
          <v-btn
            fab
            icon
            class="pa-0 ma-0"
            small
            @click.stop="dialog_topup = false"
          >
            <v-icon color="red">mdi-close</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col>
              <v-list-item>
                <v-list-item-content class="text-center">
                  <v-list-item-title>{{ detail.nama }}</v-list-item-title>
                  <v-list-item-subtitle>{{ detail.nis }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Saldo DigitCard</v-list-item-title>
                  <v-list-item-title>Saldo Total</v-list-item-title>
                </v-list-item-content>
                <v-list-item-content class="text-right">
                  <v-list-item-title
                    >Rp. {{ detail.saldo_digitcard }}</v-list-item-title
                  >
                  <v-list-item-title
                    >Rp. {{ detail.saldo_total }}</v-list-item-title
                  >
                </v-list-item-content>
              </v-list-item>
            </v-col>
            <v-col
              cols="12"
              md="6"
              order="first"
              order-md="last"
              class="text-center"
            >
              <img
                src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
                width="40%"
              />
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <!-- {{input.topup}} -->
              <v-tabs fixed-tabs v-model="input.topup.saldo_type">
                <v-tab href="#card">DigitCard</v-tab>
                <v-tab href="#total">Total</v-tab>
              </v-tabs>
            </v-col>
            <v-col cols="12" class="py-0">
              <v-text-field
                label="Nominal"
                type="number"
                v-model="input.topup.nominal"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-btn block color="primary" @click.stop="topUpSiswa">Top Up</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- DIALOG TAMBAH SISWA -->
    <v-dialog v-model="dialog_tambah_siswa" persistent width="600px">
      <v-card>
        <v-card-title
          >Tambah siswa
          <v-spacer></v-spacer>
          <v-btn
            icon=""
            @click.stop="
              dialog_tambah_siswa = false
              $refs.form_tambah_siswa.reset()
            "
            ><v-icon>mdi-close</v-icon></v-btn
          >
        </v-card-title>
        <v-card-text>
          <v-form ref="form_tambah_siswa">
            <v-row>
              <v-col cols="12" class="py-0">
                <v-text-field
                  label="Nama"
                  v-model="input.siswa.nama"
                ></v-text-field>
              </v-col>
              <v-col cols="12" class="py-0">
                <v-text-field
                  label="Nomor Induk Siswa"
                  type="number"
                  v-model="input.siswa.nis"
                  counter
                ></v-text-field>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-btn block="" color="primary" @click.stop="tambahSiswa"
            >Simpan</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- DIALOG SUKSES -->
    <v-snackbar v-model="sukses.siswa" color="primary">
      Siswa telah ditambahkan
      <v-btn dark icon small @click="sukses.siswa = false" fab>
        <v-icon>mdi-close-thick</v-icon>
      </v-btn>
    </v-snackbar>

    <!-- DIALOG ERROR -->
    <v-snackbar v-model="error.status" color="red" multi-line>
      {{ error.msg }}
      <v-btn dark icon small @click="error.status = false" fab>
        <v-icon>mdi-close-thick</v-icon>
      </v-btn>
    </v-snackbar>

    <!-- NAVIGATION DETAIL SISWA -->
    <v-navigation-drawer v-model="drawer" right app clipped bottom>
      <template v-slot:prepend>
        <v-list-item two-line>
          <v-list-item-avatar>
            <img
              src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
            />
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>{{ detail.nama }}</v-list-item-title>
            <v-list-item-subtitle>{{ detail.nis }}</v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>
            <v-btn icon color="primary"
              ><v-icon>mdi-account-edit</v-icon></v-btn
            >
          </v-list-item-action>
        </v-list-item>
        <v-divider></v-divider>
      </template>
      <template v-slot:append>
        <v-divider></v-divider>
        <v-list-item one-line>
          <v-list-item-content>
            <v-btn color="red" text small @click="drawer = !drawer"
              >Tutup</v-btn
            >
          </v-list-item-content>
        </v-list-item>
      </template>
      <v-list nav class="py-0">
        <v-list-item dense>
          <v-list-item-content>
            <v-list-item-title>Saldo total</v-list-item-title>
            <v-list-item-subtitle>
              {{ detail.saldo_total }}
            </v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>
            <v-btn
              color="primary"
              text
              small
              @click.stop="
                dialog_topup = true
                input.topup.saldo_type = 'total'
              "
              >Top up</v-btn
            >
          </v-list-item-action>
        </v-list-item>
        <v-list-item dense>
          <v-list-item-content>
            <v-list-item-title>Saldo DigitCard</v-list-item-title>
            <v-list-item-subtitle>
              {{ detail.saldo_digitcard }}
            </v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>
            <v-btn
              color="primary"
              text
              small
              @click.stop="
                dialog_topup = true
                input.topup.saldo_type = 'card'
              "
              >Top up</v-btn
            >
          </v-list-item-action>
        </v-list-item>
        <v-divider></v-divider>

        <v-list-item
          two-line
          class="py-0"
          dense
          v-for="trans in detail.transaksi"
          :key="trans.id"
        >
          <v-list-item-icon>
            <v-icon color="green" v-if="trans.mutasi_type == 'in'"
              >mdi-arrow-left</v-icon
            >
            <v-icon color="red" v-else>mdi-arrow-right</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>Rp {{ trans.nominal }}</v-list-item-title>
            <v-list-item-subtitle>
              {{ $moment(trans.created_at).format('MM-DD-YYYY LT') }}
            </v-list-item-subtitle>
            <v-list-item-subtitle>
              <span v-if="trans.saldo_type == 'card'">DigitCard</span>
              <span v-else>Total</span>
            </v-list-item-subtitle>
          </v-list-item-content>
          <!-- <v-list-item-action>
            <v-list-item-title>Admin</v-list-item-title>
          </v-list-item-action>-->
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </div>
</template>
<script>
export default {
  data() {
    return {
      table_siswa_header: [
        { text: 'Nama siswa', value: 'nama' },
        { text: 'NIS', value: 'nis' },
        { text: 'Saldo DigitCard', value: 'saldo_digitcard' },
        { text: 'Saldo Bulanan', value: 'saldo_total' },
        { text: '', value: 'id' }
      ],
      daftar_siswa: [
        {
          nama: 'Rian Ardiana Prapanca',
          nis: 'G64170085',
          saldo_digitcard: '20000',
          saldo_total: '150000',
          id: 1
        }
      ],
      dialog_tambah_siswa: false,
      dialog_topup: false,
      search: '',
      drawer: false,
      input: {
        siswa: {
          nama: null,
          nis: null
        },
        topup: {
          siswa_id: null,
          nominal: null,
          mutasi_type: 'in',
          saldo_type: 'card'
        }
      },
      detail: {
        nama: null,
        nis: null,
        saldo_digitcard: null,
        saldo_total: null,
        id: null
      },
      sukses: {
        siswa: false
      },
      error: {
        status: false,
        msg: ''
      }
    }
  },
  methods: {
    async getSiswa() {
      try {
        const siswa = await this.$axios.$get('siswa')
        this.daftar_siswa = siswa
      } catch (e) {
        console.log(e)
        this.error.status = true
        this.error.msg = e
      }
    },
    async tambahSiswa() {
      try {
        const siswa = await this.$axios({
          method: 'post',
          url: 'siswa',
          data: this.input.siswa
        })
        this.sukses.siswa = true
        this.dialog_tambah_siswa = false
        this.getSiswa()
      } catch (e) {
        console.log(e)
        this.error.status = true
        this.error.msg = e
      }
    },
    async lihatDetail(obj) {
      try {
        const siswa = await this.$axios.$get('siswa/' + obj.nis)
        this.detail = siswa
        this.input.topup.siswa_id = siswa.id
      } catch (e) {
        console.log(e)
        this.error.status = true
        this.error.msg = e
      }
    },
    async topUpSiswa() {
      try {
        const res = await this.$axios({
          method: 'post',
          url: '/transaksi',
          data: this.input.topup
        })
        console.log(res)
      } catch (e) {
        console.log(e)
        this.error.status = true
        this.error.msg = e
      }
    }
  },
  computed: {
    filteredSiswa: function() {
      return this.daftar_siswa.filter(siswa => {
        return (
          siswa.nama.toLowerCase().match(this.search.toLowerCase()) ||
          siswa.nis.toLowerCase().match(this.search.toLowerCase())
        )
      })
    }
  },
  mounted() {
    this.getSiswa()
  }
}
</script>
<style scoped>
.bg-siswa {
  background: linear-gradient(
      rgba(255, 255, 255, 0.9),
      rgba(255, 255, 255, 0.8)
    ),
    url('~assets/img/bg-siswa.svg') no-repeat;
  background-position: center center;
  background-attachment: fixed;
  background-size: contain;
  min-height: 85vh;
}
.add-btn {
  position: fixed;
  bottom: 0;
  right: 0;
}
</style>

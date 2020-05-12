<template>
  <div class="bg-product">
    <!-- Main Content -->
    <v-layout column>
      <!-- Search Box -->
      <v-flex md12 xs12 lg12 sm12>
        <v-text-field
          solo
          prepend-inner-icon="mdi-card-search-outline"
          label="Cari produk"
          hide-details
          v-model="search"
        ></v-text-field>
      </v-flex>

      <!-- List Produk -->
      <v-flex>
        <v-list class="transparent">
          <template v-if="produk.length == 0">
            <v-row justify="center">
              <v-avatar size="100px">
                <v-icon size="50px" color="primary" class="mdi-spin">mdi-loading</v-icon>
              </v-avatar>
            </v-row>
          </template>
          <!-- <pre>{{produk}}</pre> -->
          <template v-for="prod in filteredProduk">
            <div :key="prod.id">
              <v-list-item @click.stop="editProduk(prod)">
                <v-list-item-avatar>
                  <div class="headline primary--text">{{ prod.jumlah }}</div>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title class="font-weight-bold">{{ prod.nama }}</v-list-item-title>
                  <v-list-item-subtitle>{{ prod.nomor }}</v-list-item-subtitle>
                  <v-list-item-subtitle>
                    Beli : Rp. {{ prod.harga_beli }}
                    <span
                      class="float-right"
                    >Jual : Rp. {{ prod.harga_jual }}</span>
                  </v-list-item-subtitle>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn icon color="primary">
                    <v-icon>mdi-qrcode-edit</v-icon>
                  </v-btn>
                </v-list-item-action>
                <!-- <v-list-item-content class="text-right">
                  <v-list-item-subtitle class="font-weight-medium">Beli : Rp. {{ prod.harga_beli }}</v-list-item-subtitle>
                  <v-list-item-subtitle class="font-weight-medium">Jual : Rp. {{ prod.harga_jual }}</v-list-item-subtitle>
                </v-list-item-content>-->
              </v-list-item>
              <v-divider></v-divider>
            </div>
          </template>
        </v-list>
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

    <!-- MODAL -->
    <!-- DIALOG TAMBAH PRODUK -->
    <v-dialog v-model="dialog_tambah_produk" persistent width="600px">
      <v-card>
        <v-card-title>
          Tambah produk
          <v-spacer></v-spacer>
          <v-btn icon @click.stop="dialog_tambah_produk = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-form ref="form_tambah_produk">
            <v-row>
              <v-col cols="12">
                <v-text-field v-model="tambah_produk.nama" label="Nama produk" hide-details></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="tambah_produk.nomor"
                  label="Nomor produk"
                  hide-details
                  type="number"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="tambah_produk.harga_beli"
                  label="Harga pokok"
                  type="number"
                  hide-details
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="tambah_produk.harga_jual"
                  label="Harga jual"
                  type="number"
                  hide-details
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="tambah_produk.jumlah"
                  label="Stok awal"
                  type="number"
                  hide-details
                ></v-text-field>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-btn block color="primary" @click.stop="storeProduk">Simpan</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- DIALOG EDIT PRODUK -->
    <v-dialog v-model="dialog_edit_produk" persistent width="600px">
      <v-card>
        <v-card-title>
          Edit produk
          <v-spacer></v-spacer>
          <v-btn icon @click.stop="dialog_edit_produk = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-form ref="form_edit_produk">
            <v-row>
              <v-col cols="12">
                <v-text-field v-model="edit_produk.nama" label="Nama produk" hide-details></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="edit_produk.nomor"
                  label="Nomor produk"
                  hide-details
                  type="number"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="edit_produk.harga_beli"
                  label="Harga pokok"
                  type="number"
                  hide-details
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="edit_produk.harga_jual"
                  label="Harga jual"
                  type="number"
                  hide-details
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field v-model="edit_produk.jumlah" label="Stok" type="number" hide-details></v-text-field>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-btn block color="primary" @click.stop="updateProduk">Simpan</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- FAB -->
    <div class="add-btn">
      <v-btn fab color="primary" class="ma-4" @click.stop="dialog_tambah_produk = true">
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialog_tambah_produk: false,
      dialog_edit_produk: false,
      search: '',
      produk: [],
      tambah_produk: {
        nama: null,
        nomor: null,
        harga_beli: null,
        harga_jual: null,
        jumlah: null
      },
      edit_produk: {
        id: null,
        nama: '',
        nomor: '',
        harga_beli: null,
        harga_jual: null,
        jumlah: null
      },
      sukses: {
        status: false,
        msg: null
      },
      error: {
        status: false,
        msg: null
      }
    }
  },
  methods: {
    async storeProduk() {
      try {
        const res = await this.$axios({
          method: 'post',
          url: 'produk',
          data: this.tambah_produk
        })
        console.log(res)
        this.sukses.status = true
        this.sukses.msg = 'Produk berhasil disimpan'
        this.dialog_tambah_produk = false
        this.getProduk()
        this.$refs.form_tambah_produk.reset()
      } catch (e) {
        console.error(e)
        this.error.status = true
        this.error.msg = 'Hmmm ada yang salah deh'
      }
    },
    async updateProduk() {
      try {
        const res = await this.$axios({
          method: 'put',
          url: 'produk/' + this.edit_produk.id,
          data: this.edit_produk
        })
        console.log(res)
        this.sukses.status = true
        this.sukses.msg = 'Produk berhasil disimpan'
        this.dialog_edit_produk = false
        this.getProduk()
        this.$refs.form_edit_produk.reset()
      } catch (e) {
        console.error(e)
        this.error.status = true
        this.error.msg = 'Hmmm ada yang salah deh'
      }
    },
    async getProduk() {
      try {
        const res = await this.$axios.get('produk')
        this.produk = res.data
        // console.log(res)
      } catch (e) {
        console.error(e)
      }
    },
    editProduk(obj) {
      this.dialog_edit_produk = true
      Object.assign(this.edit_produk, obj)
    }
  },
  computed: {
    filteredProduk: function() {
      return this.produk.filter(prod => {
        return prod.nama.match(this.search) || prod.nomor.match(this.search)
      })
    }
  },
  mounted() {
    this.getProduk()
  }
}
</script>

<style scoped>
.add-btn {
  position: fixed;
  bottom: 0;
  right: 0;
}
.bg-product {
  background: linear-gradient(
      rgba(255, 255, 255, 0.9),
      rgba(255, 255, 255, 0.8)
    ),
    url('~assets/img/bg-product.svg') no-repeat;
  background-position: center right;
  background-size: contain;
  min-height: 85vh;
  background-attachment: fixed;
}
</style>

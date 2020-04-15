<template>
  <div>
    <div>
      <v-row>
        <v-col cols="12">
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
                dense
                hide-details
                v-model="search"
                prepend-inner-icon="mdi-account-search"
                color="green"
              ></v-text-field>
              <v-data-table :headers="table_siswa_header" :items="daftar_siswa" :search="search">
                <template v-slot:item.id="{ item }">
                  <v-btn text small color="primary" @click="drawer = !drawer">Lihat detail</v-btn>
                </template>
              </v-data-table>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
      <v-dialog v-model="dialog_tambah_siswa" persistent width="600px">
        <v-card>
          <v-card-title>Tambah siswa</v-card-title>
          <v-card-text>
            <v-row>
              <v-col cols="12" class="py-0">
                <v-text-field color="green" label="Nama"></v-text-field>
              </v-col>
              <v-col cols="12" class="py-0">
                <v-text-field color="green" label="NISN"></v-text-field>
              </v-col>
            </v-row>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn text @click.stop="dialog_tambah_siswa = false">Batal</v-btn>
            <v-btn text color="green">Simpan</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-navigation-drawer v-model="drawer" right app clipped bottom>
        <template v-slot:prepend>
          <v-list-item two-line>
            <v-list-item-avatar>
              <img
                src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
              />
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>Rian Ardiana Prapanca</v-list-item-title>
              <v-list-item-subtitle>G64170085</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
          <v-divider></v-divider>
        </template>
        <template v-slot:append>
          <v-divider></v-divider>
          <v-list-item one-line>
            <v-list-item-content>
              <v-btn color="red" text small @click="drawer = !drawer">Tutup</v-btn>
            </v-list-item-content>
          </v-list-item>
        </template>
        <v-list nav class="py-0">
          <v-list-item dense>
            <v-list-item-content>
              <v-list-item-title>Saldo total</v-list-item-title>
              <v-list-item-subtitle>Rp. 150000</v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn color="primary" text small>Top up</v-btn>
            </v-list-item-action>
          </v-list-item>
          <v-list-item dense>
            <v-list-item-content>
              <v-list-item-title>Saldo DigitCard</v-list-item-title>
              <v-list-item-subtitle>Rp. 20000</v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn color="primary" text small>Top up</v-btn>
            </v-list-item-action>
          </v-list-item>
          <v-divider></v-divider>

          <v-list-item two-line class="py-0" dense>
            <v-list-item-icon>
              <v-icon color="red">mdi-arrow-left</v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title>Rp 5000</v-list-item-title>
              <v-list-item-subtitle>11/11/11</v-list-item-subtitle>
              <v-list-item-subtitle>DigitCard</v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-list-item-title>Admin</v-list-item-title>
            </v-list-item-action>
          </v-list-item>
          <v-list-item two-line class="py-0" dense>
            <v-list-item-icon>
              <v-icon color="green">mdi-arrow-right</v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title>Rp 5000</v-list-item-title>
              <v-list-item-subtitle>11/11/11</v-list-item-subtitle>
              <v-list-item-subtitle>Saldo total</v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-list-item-title>Pembina</v-list-item-title>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      table_siswa_header: [
        { text: 'Nama siswa', value: 'nama' },
        { text: 'NISN', value: 'nisn' },
        { text: 'Saldo DigitCard', value: 'saldo_kartu' },
        { text: 'Saldo Bulanan', value: 'saldo_total' },
        { text: '', value: 'id' }
      ],
      daftar_siswa: [
        {
          nama: 'Rian Ardiana Prapanca',
          nisn: 'G64170085',
          saldo_kartu: '20000',
          saldo_total: '150000',
          id: 1
        }
      ],
      dialog_tambah_siswa: false,
      search: '',
      drawer: false
    }
  }
}
</script>

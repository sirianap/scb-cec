<template>
  <v-layout>
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>
            Daftar siswa
            <v-spacer></v-spacer>
            <v-btn color="green" dark class="px-5" @click.stop="dialog_tambah_siswa = true">Tambah</v-btn>
          </v-card-title>
          <v-card-text>
            <v-text-field
              outlined
              label="Cari siswa"
              dense
              hide-details
              prepend-inner-icon="mdi-account-search"
              color="green"
            ></v-text-field>
            <v-data-table :headers="table_siswa_header" :items="daftar_siswa"></v-data-table>
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
  </v-layout>
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
      dialog_tambah_siswa: false
    }
  }
}
</script>

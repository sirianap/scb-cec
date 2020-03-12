<template>
  <v-layout justify-center align-center>
    <v-row>
      <v-col cols="12" md="4">
        <v-card>
          <v-card-title>Hari ini</v-card-title>
          <v-card-text>
            <span class="display-2">Rp. 4.000.000</span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="4">
        <v-card>
          <v-card-title>Minggu ini</v-card-title>
          <v-card-text>
            <span class="display-2">Rp. 4.000.000</span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="4">
        <v-card>
          <v-card-title>Bulan ini</v-card-title>
          <v-card-text>
            <span class="display-2">Rp. 4.000.000</span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12">
        <v-card>
          <v-card-title>Rekapan Pembayaran</v-card-title>
          <v-data-table></v-data-table>
        </v-card>
      </v-col>
      <v-col cols="12">
        <v-card>
          <v-card-title>Riwayat Transaksi</v-card-title>
          <v-data-table></v-data-table>
        </v-card>
      </v-col>
    </v-row>
  </v-layout>
</template>

<script>
export default {}
</script>

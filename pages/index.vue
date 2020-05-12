<template>
  <div class="bg-dashboard">
    <v-layout column>
      <v-flex>
        <v-row>
          <v-col cols="12" md="3" class="pt-0">
            <v-menu v-model="menu1" :close-on-content-click="false" max-width="290" offset-y>
              <template v-slot:activator="{ on }">
                <v-text-field
                  :value="dateRangeText"
                  clearable
                  prepend-icon="mdi-calendar"
                  label="Tanggal"
                  readonly
                  hide-details
                  v-on="on"
                  @click:clear="dates = []"
                ></v-text-field>
              </template>
              <v-date-picker v-model="dates" @change="menu1 = false" range no-title></v-date-picker>
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
                  <v-list-item-title class="primary--text">Pembayaran masuk</v-list-item-title>
                  <v-list-item-subtitle class="display-1">Rp 5000</v-list-item-subtitle>
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
                  <v-list-item-title class="primary--text">Keuntungan</v-list-item-title>
                  <v-list-item-subtitle class="display-1">Rp 5000</v-list-item-subtitle>
                </v-list-item-content>
                <v-list-item-icon>
                  <v-icon size="50px" color="primary">mdi-flower-tulip-outline</v-icon>
                </v-list-item-icon>
              </v-list-item>
            </v-card>
          </v-col>
          <v-col cols="12" md="4">
            <v-card>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title class="primary--text">Jumlah Transaksi</v-list-item-title>
                  <v-list-item-subtitle class="display-1">200</v-list-item-subtitle>
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
              <v-data-table :headers="header"></v-data-table>
            </v-card>
          </v-col>
        </v-row>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dates: [],
      menu1: false,
      header: [
        { text: 'Waktu' },
        { text: 'Pembeli' },
        { text: 'Nominal' },
        { text: 'PIC' }
      ]
    }
  },
  computed: {
    dateRangeText() {
      return this.dates.join(' ~ ')
    }
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
</style>

<template>
  <v-app dark>
    <v-navigation-drawer v-model="drawer" fixed app clipped>
      <template v-slot:prepend>
        <v-list-item class="pt-2">
          <v-container text-center>
            <v-img
              src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/Logo-SCB_Hitam.png"
            ></v-img>
          </v-container>
        </v-list-item>
      </template>
      <template v-slot:append>
        <v-list-item class="pt-2">
          <v-list-item-content>
            <v-list-item-subtitle>Dikembangkan oleh</v-list-item-subtitle>
            <v-list-item-title>RianAP Ilmu Komputer 54 IPB</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </template>
      <v-list>
        <v-list-item v-for="(item, i) in items" :key="i" :to="item.to" router exact color="green">
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar clipped-left fixed app color="green" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-spacer />
      <img
        src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
        height="40px"
        width="40px"
      />
      <!-- <v-toolbar-title v-text="title" /> -->
      <v-spacer />
      <v-btn icon>
        <v-icon>mdi-face</v-icon>
      </v-btn>
    </v-app-bar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    <v-footer fixed app>
      <span>Web sedang dalam pengembangan</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: true,
      items: [
        {
          icon: 'mdi-view-dashboard',
          title: 'Dashboard',
          to: '/'
        },
        {
          icon: 'mdi-account-group',
          title: 'Daftar siswa',
          to: '/daftar-siswa'
        },
        {
          icon: 'mdi-cash-100',
          title: 'Pembayaran',
          to: '/pembayaran'
        }
      ],
      title: 'Sistem Pembayaran CEC'
    }
  }
}
</script>

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
      <v-list flat>
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
    <v-app-bar clipped-left clipped-right fixed app color="green" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-spacer />
      <img
        src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
        height="40px"
        width="40px"
      />
      <!-- <v-toolbar-title v-text="title" /> -->
      <v-spacer />
      <v-menu transition="slide-y-transition" offset-x offset-y bottom>
        <template v-slot:activator="{ on }">
          <v-btn icon v-on="on">
            <v-icon>mdi-account</v-icon>
          </v-btn>
        </template>
        <v-card>
          <v-card-text>
            <v-list-item>
              <v-list-item-avatar>
                <img
                  src="https://www.cendekiabaznas.sch.id/wp-content/uploads/2017/12/cropped-Logo-SCB-300x300.png"
                  width="100%"
                />
              </v-list-item-avatar>
              <v-list-item-content>
                <!-- <pre>{{$auth.user}}</pre> -->
                <v-list-item-title>{{$auth.user.name}}</v-list-item-title>
                <v-list-item-subtitle>{{$auth.user.email}}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn text color="primary" @click.stop="logout">logout</v-btn>
          </v-card-actions>
        </v-card>
      </v-menu>
    </v-app-bar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    <!-- <v-footer fixed app>
      <span>Web sedang dalam pengembangan</span>
    </v-footer>-->
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
          title: 'Siswa',
          to: '/siswa'
        },
        {
          icon: 'mdi-cart',
          title: 'Produk',
          to: '/produk'
        },
        {
          icon: 'mdi-cash-100',
          title: 'Pembayaran',
          to: '/pembayaran'
        },
        {
          icon: 'mdi-tools',
          title: 'Admin',
          to: '/administrator'
        }
      ],
      title: 'Sistem Pembayaran CEC'
    }
  },
  methods: {
    async logout() {
      try {
        res = await this.$auth.logout()
        console.log(res)
      } catch (e) {
        console.error(e)
      }
    }
  }
}
</script>

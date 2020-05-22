<template>
  <div>
    <v-layout>
      <v-flex>
        <v-card>
          <v-card-title>
            Pengguna
            <v-spacer></v-spacer>
            <v-btn color="primary" @click.stop="dialog_registrasi = true">Tambah</v-btn>
          </v-card-title>
          <v-data-table :items="users" :headers="header_user">
            <template v-slot:item.id="{item}">
              <v-btn text @click.stop="editUser(item)">Edit</v-btn>
            </template>
          </v-data-table>
        </v-card>
      </v-flex>
    </v-layout>
    <v-dialog v-model="dialog_registrasi" width="450px" persistent>
      <v-card>
        <v-card-title>Tambah pengguna</v-card-title>
        <v-card-text>
          <v-form>
            <v-row>
              <v-col cols="12 py-0">
                <v-text-field label="Nama" v-model="register.name"></v-text-field>
              </v-col>
              <v-col cols="12 py-0">
                <v-text-field label="Username" v-model="register.email"></v-text-field>
              </v-col>
              <v-col cols="12 py-0">
                <v-text-field label="Password" v-model="register.password" type="password"></v-text-field>
              </v-col>
              <v-col cols="12 py-0">
                <v-select
                  label="Role"
                  v-model="register.role"
                  :items="['sysadmin','admin','pembina','kasir']"
                ></v-select>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click.stop="dialog_registrasi = false">keluar</v-btn>
          <v-btn text color="primary" @click.stop="storeUser">simpan</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialog_edit" width="450px" persistent>
      <v-card>
        <v-card-title>Edit pengguna</v-card-title>
        <v-card-text>
          <v-form>
            <v-row>
              <v-col cols="12 py-0">
                <v-text-field label="Nama" v-model="edit.name"></v-text-field>
              </v-col>
              <v-col cols="12 py-0">
                <v-text-field label="Username" v-model="edit.email"></v-text-field>
              </v-col>
              <v-col cols="12 py-0">
                <v-text-field label="Password" v-model="edit.password" type="password"></v-text-field>
              </v-col>
              <v-col cols="12 py-0">
                <v-select
                  label="Role"
                  v-model="edit.role"
                  :items="['sysadmin','admin','pembina','kasir']"
                ></v-select>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click.stop="dialog_edit = false">keluar</v-btn>
          <v-btn text color="primary" @click.stop="update()">simpan</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
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
      header_user: [
        { text: 'Nama', value: 'name' },
        { text: 'Username', value: 'email' },
        { text: 'Role', value: 'role' },
        { text: '', value: 'id' }
      ],
      users: [],
      dialog_registrasi: false,
      dialog_edit: false,
      register: {
        name: '',
        email: '',
        password: '',
        role: 'kasir'
      },
      edit: {
        id: '',
        name: '',
        email: '',
        password: null,
        role: ''
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
    async getUsers() {
      try {
        const users = await this.$axios.$get('users')
        console.log(users)
        this.users = users
      } catch (e) {
        console.error(e)
      }
    },
    async storeUser() {
      try {
        const res = await this.$axios({
          method: 'post',
          url: 'register',
          data: this.register
        })
        this.sukses.status = true
        this.sukses.msg = 'Pengguna berhasil ditambahkan'
        console.log(res)
      } catch (e) {
        console.error(e)
        this.error.status = true
        this.error.msg = 'Ada yang salah nih kayanya'
      }
    },
    async update() {
      try {
        const res = await this.$axios({
          method: 'post',
          url: 'user/' + this.edit.id,
          data: this.edit
        })
        console.log(res)
        this.sukses.status = true
        this.sukses.msg = 'Pengguna berhasil diupdate'
        this.dialog_edit = false
      } catch (e) {
        console.error(e)
        this.error.status = true
        this.error.msg = 'Ada yang salah nih kayanya'
      }
    },
    editUser(obj) {
      Object.assign(this.edit, obj)
      this.dialog_edit = true
    }
  },
  mounted() {
    this.getUsers()
  }
}
</script>

<style>
</style>
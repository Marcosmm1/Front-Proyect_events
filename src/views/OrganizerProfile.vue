<template>
  <v-container fluid fill-height>
    <v-row id="background">
      <v-col class="reboot-col" >
      <img src="@/assets/Publicity2.png" class="reboot"/>
      </v-col>
       <v-col cols="6">
  <v-row class="d-flex justify-center mx-auto">
     <v-col cols="6" sm="10" lg="7" class="mx-auto text-end">
      <div align="start">
      <h2 class="display-1 mb-10 font-weight-medium">EDIT YOUR INFORMATION</h2>
      </div>
      <v-btn text @click="activeEdit">Edit</v-btn>
      <v-form ref="form">
        <span :style="{'color' : 'green'}">{{updated}}</span>
        <v-text-field :disabled="edit" v-model="userdb.first_name" label="Name"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.last_name" label="Last Name"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.email" label="Email"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.mobile" label="Mobile"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.social_fb" label="Facebook"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.social_it" label="Instagram"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.VATIN" label="VATIN"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.social_lk" label="Linkedin"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.business_name" label="business_name"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.organizer_info" label="organizer_info"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.address" label="address"></v-text-field>
        <v-text-field :disabled="edit" v-model="userdb.zip_code" label="zip_code"></v-text-field>
      </v-form>
      </v-col>
<v-col cols="4" justify="start">
  <h2>Change Password</h2>
 <v-text-field
            v-model="password"
            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            :rules="[rules.required, rules.min]"
            :type="show1 ? 'text' : 'password'"
            label="Actual password"
            hint="At least 8 characters"
            @click:append="show1 = !show1"
          ></v-text-field>
           <v-text-field
            v-model="newPassword"
            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            :rules="[rules.required, rules.min]"
            :type="show1 ? 'text' : 'password'"
            label="New password"
            hint="At least 8 characters"
            @click:append="show1 = !show1"
          ></v-text-field>
           <v-text-field
            v-model="confirmPassword"
            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            :rules="[rules.required, rules.min]"
            :type="show1 ? 'text' : 'password'"
            label="Repeat new password"
            hint="At least 8 characters"
            @click:append="show1 = !show1"
          ></v-text-field>
        <v-btn :disabled="checkFormPsw" :class="$vuetify.breakpoint.smAndDown ? 'mt-10' : ''" text color="blue" outlined @click='updatePsw'>Update password</v-btn> <br>
        <br>
        <span :style="colorPassword ? 'color: green' : 'color: red'"> {{msg}} </span>
        </v-col>
        </v-row>
        <v-row>
        <v-col class="text-center">
        <v-btn :disabled="edit" text color="blue" outlined class="mx-5" @click ='editProfile'>Edit your information</v-btn>
        <v-btn :class="$vuetify.breakpoint.xsOnly ? 'mt-10' : ''" text color="red" outlined @click='deleteProfile'>Delete your account</v-btn>
      </v-col>
      </v-row>
      </v-col>
      <v-col class="reboot-col">
      <img src="@/assets/Publicity3.png" class="reboot" style="right: 0px"/>
      </v-col>
      </v-row>
      </v-container>
</template>

<script>
import API from '../services/App'
export default {
  data: () => ({
    userdb: {},
    show1: false,
    colorPassword: false,
    password: '',
    newPassword: '',
    confirmPassword: '',
    msg: '',
    updated: '',
    edit: true,
    rules: {
      required: value => !!value || 'Required.',
      min: v => v.length >= 8 || 'Min 8 characters'
    }
  }),
  created () {
    API.getUserInfo().then(response => {
      return (this.userdb = response)
    })
  },
  methods: {
    activeEdit () {
      this.edit = false
    },
    async editProfile () {
      this.edit = true
      const userUpdate = {
        first_name: this.userdb.first_name,
        last_name: this.userdb.last_name,
        email: this.userdb.email,
        mobile: this.userdb.mobile,
        VATIN: this.userdb.VATIN,
        business_name: this.userdb.business_name,
        social_fb: this.userdb.social_fb,
        social_it: this.userdb.social_it,
        social_lk: this.userdb.social_lk
      }
      await API.updateProfile(userUpdate)
      this.updated = 'Profile updated'
      if (this.newPassword) {
        this.updatePsw()
      }
    },
    deleteProfile (user) {
      API.deleteProfile(user)
    },
    updatePsw () {
      const newPassword = {
        actualPassword: this.password,
        newPassword: this.confirmPassword
      }
      API.changePassword(newPassword).then(response => {
        if (response.error) {
          this.msg = 'Password Incorrect'
          this.colorPassword = false
        } else {
          this.msg = 'Password has been updated please log in again'
          this.colorPassword = true
        }
      })
    }
  },
  computed: {
    checkFormPsw () {
      if (this.password.length === 0 ||
        this.newPassword.length === 0 ||
        this.confirmPassword.length === 0 ||
        this.newPassword !== this.confirmPassword) {
        return true
      }
      return false
    }
  }
}
</script>

<style lang="scss" scoped>
*{
  font-size: 20px;
}
#background{
background-color: rgba(221, 230, 233, 0.657);
margin-top: -1%;
margin-bottom: -1%;
min-height: calc(100vh - 6vh);
}
.reboot-col {
  padding: 0px;
  margin-top: -20px;
}
.reboot {
  top: 0px;
  position: fixed;
  height: 100vh;
  width: 8vw;
}
.container-lg {
   height: 100vh;
   margin-bottom: 5vh;
}
</style>

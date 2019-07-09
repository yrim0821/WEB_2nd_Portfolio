<template>
  <v-layout align-center justify-center row fill-height>
    <v-flex xs5 text-xs-center>

      <div style="margin-top:100px"></div>



      <v-layout align-center justify-center row fill-height elevation-5 style="min-height:300px;" white pa-4>

        <v-flex xs12 text-xs-center>

          <div style="padding:10px;">
            <v-text-field v-model="loginEmail" label="Email" placeholder="Placeholder"></v-text-field>
            <v-text-field v-model="loginPassword" label="Password" placeholder="Placeholder" type="password"></v-text-field>
          </div>

          <div style="margin-top:20px"></div>

          <v-btn round color="#20aa49" dark v-on:click="loginWithMail" style="width:100%;"><v-icon size="25" class="mr-2">mail</v-icon> Mail 로그인</v-btn>
          <v-btn round color="#df4a31" dark v-on:click="loginWithGoogle" style="width:100%;"><v-icon size="25" class="mr-2">fa-google</v-icon> Google 로그인</v-btn>
          <v-btn round color="#4267B2" dark v-on:click="loginWithFacebook" style="width:100%;"><v-icon size="25" class="mr-2">fa-facebook</v-icon> Google 로그인</v-btn>

          <v-dialog v-model="dialog" width="500">
            <template v-slot:activator="{ on }">
              <v-btn round color="#888888" dark v-on="on" style="width:100%;"><v-icon size="25" class="mr-2">person</v-icon>회원가입</v-btn>
            </template>

            <v-card>
              <v-card-title class="headline grey lighten-2" primary-title>Sign up</v-card-title>
              <v-card-text>
                <form>
                  <v-text-field v-model="signupEmail" label="Email" placeholder="Placeholder"></v-text-field>
                  <v-text-field v-model="signupPassword" label="Password" placeholder="Placeholder"></v-text-field>
                </form>
              </v-card-text>
              <v-divider></v-divider>

              <v-card-actions>
                <v-spacer></v-spacer>
                  <v-btn color="primary" flat v-on:click="signUp()">SignUp</v-btn>
                  <v-btn color="primary" flat @click="dialog = false">close</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-flex>
      </v-layout>

      <div style="margin-top:20px"></div>

    </v-flex>
  </v-layout>
</template>

<script>
import FirebaseService from '@/services/FirebaseService'
import firebase from 'firebase/app'

export default {
   name: 'LoginPage',
   data() {
      return {
      loginEmail: '',
      loginPassword: '',
      signupEmail: '',
      signupPassword: '',
      dialog: false,
      user: firebase.auth().currentUser
    }
   },
   components: {},
   methods: {
    async loginWithMail(){
      firebase.auth().signInWithEmailAndPassword(this.loginEmail, this.loginPassword)
      .then((user)=>{

        this.$store.state.accessToken = user.user.refreshToken
        this.$store.state.user = user.user.email
        alert(this.$store.state.user + " 님 로그인 되었습니다")
        this.$router.replace('/')

      })
      .catch((error)=>{
        alert(error)
      })

    },
      async loginWithGoogle() {
         const result = await FirebaseService.loginWithGoogle()
         this.$store.state.accessToken = result.credential.accessToken
         this.$store.state.user = result.user
      alert(this.$store.state.user.displayName + " 님 로그인 되었습니다")
      this.$router.replace('/')
      },
    async loginWithFacebook() {
         const result = await FirebaseService.loginWithFacebook()
         this.$store.state.accessToken = result.credential.accessToken
         this.$store.state.user = result.user
      alert(this.$store.state.user.displayName + " 님 로그인 되었습니다")
      this.$router.replace('/')
      },
    signUp(){
      firebase.auth().createUserWithEmailAndPassword(this.signupEmail, this.signupPassword)
      .then((user)=>{
        alert("Your account has been created!")
        this.$router.replace('/')
      })
      .catch((error)=>{
        alert(error)
      })
    }

   },
   mounted() {
    this.user = firebase.auth().currentUser;
   }
}
</script>

<!--
<template>
  <div>
    <ImgBanner imgSrc="https://source.unsplash.com/5brvJbR1Pn8/1600x900">
      <div style="line-height:1.2em;font-size:1.2em;" slot="text">Sign Up</div>
    </ImgBanner>
    <v-container>


      <v-layout>
        <form>
          <v-text-field v-model="email" label="Email" placeholder="Placeholder"></v-text-field>
          <v-text-field v-model="password" type="password" label="Password" placeholder="Placeholder"></v-text-field>
          <v-btn v-on:click="signUp()">SignUp submit</v-btn>
        </form>
      </v-layout>

    </v-container>
  </div>
</template>

<script>
import ImgBanner from '../components/ImgBanner'
import FirebaseService from '@/services/FirebaseService'
import firebase from 'firebase/app'

export default {
   name: 'SignUpPage',
  data(){
    return{
      email: '',
      password: ''
    }
  },
   components: {
      ImgBanner,
   },
  mounted(){

  },
  methods:{
    signUp(){
      firebase.auth().createUserWithEmailAndPassword(this.email, this.password)
      .then((user)=>{
        alert("Your account has been created!")
        this.$router.replace('login')
      })
      .catch((error)=>{
        alert(error)
      })

    }
  }
}
</script>
-->

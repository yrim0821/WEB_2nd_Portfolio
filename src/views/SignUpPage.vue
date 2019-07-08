<template>
  <div>
    <ImgBanner imgSrc="https://source.unsplash.com/5brvJbR1Pn8/1600x900">
      <div style="line-height:1.2em;font-size:1.2em;" slot="text">Sign Up</div>
    </ImgBanner>
    <v-container>


      <v-layout>
        <form>
          <v-text-field label="Regular" placeholder="Placeholder"></v-text-field>
          <v-text-field label="Regular" placeholder="Placeholder"></v-text-field>
          <v-btn v-on:click="">SignUp submit</v-btn>

        </form>
      </v-layout>

    </v-container>
  </div>
</template>

<script>
import ImgBanner from '../components/ImgBanner'
import FirebaseService from '@/services/FirebaseService'

export default {
	name: 'SignUpPage',
  data(){
    return{}
  },
	components: {
		ImgBanner,
	},
  mounted(){

  },
  methods:{

  }
}
</script>

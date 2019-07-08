<template>
  <div>
    <ImgBanner imgSrc="https://source.unsplash.com/5brvJbR1Pn8/1600x900">
      <div style="line-height:1.2em;font-size:1.2em;" slot="text">Post</div>
    </ImgBanner>
    <v-container>

      <!-- Post -->
      <v-layout>
        <v-flex xs12>
          <PostList :limits="6" :load-more="true"></PostList>
        </v-flex>

        <form>
          <v-btn v-on:click="postPost('Post Test Title', 'Post Test Body')">Post submit</v-btn>

        </form>
      </v-layout>

    </v-container>
  </div>
</template>

<script>
import ImgBanner from '../components/ImgBanner'
import PostList from '../components/PostList'
import FirebaseService from '@/services/FirebaseService'

export default {
	name: 'PostPage',
  data(){
    return{}
  },
	components: {
		ImgBanner,
		PostList,
	},
  mounted(){

  },
  methods:{
    async postPost(title, body){
      await FirebaseService.postPost(title, body)
    }
  }
}
</script>

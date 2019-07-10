<template>
    <div>
        <ImgBanner>
        <div style="line-height:1.2em;font-size:1.2em;" slot="text">Post</div>
        </ImgBanner>

        <v-container>
            <form>
                <v-text-field v-model="title" placeholder="제목을 입력해주세요."></v-text-field>
                <markdown-editor v-model="body" ref="markdownEditor"></markdown-editor>
                <v-btn v-on:click="postPost(title, body)">submit</v-btn>
                <v-btn @click='$router.go(-1)'>back</v-btn>
            </form>
        </v-container>
    </div>
</template>

<script>
    import ImgBanner from '../components/ImgBanner'
    import markdownEditor from 'vue-simplemde/src/markdown-editor'
    import FirebaseService from '@/services/FirebaseService'

    export default {
      name: 'PostWriter',
      data(){
        return{
          title: '',
          body: ''
        }
      },
      components: {
          ImgBanner,
          markdownEditor
      },
      mounted(){

      },
      methods:{
        async postPost(title, body){
          if(title == ''){
            alert("제목을 입력해주세요")
          }else if(body == ''){
            alert("내용을 입력해주세요")
          }else{
            await FirebaseService.postPost(title, body)
            alert("post가 작성되었습니다.")
            this.$router.replace('/post')
          }

        }
      }
    }
  </script>

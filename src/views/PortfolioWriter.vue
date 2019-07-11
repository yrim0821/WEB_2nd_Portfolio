<template>
    <div>
        <ImgBanner>
        <div style="line-height:1.2em;font-size:1.2em;" slot="text" class="notranslate">Portfolio</div>
        </ImgBanner>

        <v-container>
            <form>
                <v-text-field v-model="title" placeholder="제목을 입력해주세요."></v-text-field>
                <markdown-editor v-model="body" ref="markdownEditor"></markdown-editor>
                <v-btn v-on:click="postPortfolio(title, body, img)">submit</v-btn>
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
      name: 'PortfolioWriter',
      data(){
        return{
          title: '',
          body: '',
          img: 'https://source.unsplash.com/random'
        }
      },
      components: {
          ImgBanner,
          markdownEditor
      },
      mounted(){

      },
      methods:{
        async postPortfolio(title, body, img){
          if(title == ''){
            alert("제목을 입력해주세요")
          }else if(body == ''){
            alert("내용을 입력해주세요")
          }else{
            await FirebaseService.postPortfolio(title, body, img)
            alert("portfolio가 작성되었습니다.")
            this.$router.replace('/portfolio')
          }

        }
      }
    }
  </script>

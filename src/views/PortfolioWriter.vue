<template>
  <div class="notranslate">
    <ImgBanner>
      <div style="line-height:1.2em;font-size:1.2em;" slot="text">Portfolio</div>
    </ImgBanner>

    <v-container>
      <form>
        <v-text-field v-model="title" placeholder="제목을 입력해주세요."></v-text-field>

        <v-flex xs12 class="text-xs-center text-sm-center text-md-center text-lg-center">
          <img :src="imageUrl" height="150" v-if="imageUrl"/>
          <v-text-field label="Select Image" @click='pickFile' v-model='imageName' prepend-icon='attach_file'></v-text-field>
          <input type="file" style="display: none"	ref="image" accept="image/*" @change="onFilePicked">
        </v-flex>

        <v-textarea solo v-model="body"></v-textarea>

        <v-flex  xs12 text-xs-center round my-5>
          <v-btn color="info" v-on:click="postPortfolio(title, body, img)">submit</v-btn>
          <v-btn color="info" @click='$router.go(-1)'>back</v-btn>
        </v-flex>

      </form>
    </v-container>
  </div>
</template>

<script>
import ImgBanner from '../components/ImgBanner'
import markdownEditor from 'vue-simplemde/src/markdown-editor'
import FirebaseService from '@/services/FirebaseService'
import $ from 'jquery'
export default {
  name: 'PortfolioWriter',
  data(){
    return{
      title: '',
      body: '',
      img: '',
      imageName: '',
      imageUrl: '',
      imageFile: '',
      selectedFile: null,
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
        alert("portfolio가 작성되었습니다."+img)
        this.$router.replace('/portfolio')
      }

    },
    pickFile () {
      this.$refs.image.click ()
    },
    onFilePicked (e) {
      const files = e.target.files
      if(files[0] !== undefined) {
        this.imageName = files[0].name
        if(this.imageName.lastIndexOf('.') <= 0) {
          return
        }
        const fr = new FileReader ()
        fr.readAsDataURL(files[0])
        fr.addEventListener('load', () => {
          this.imageUrl = fr.result
          this.imageFile = files[0] // this is an image file that can be sent to server...
        })
      } else {
        this.imageName = ''
        this.imageFile = ''
        this.imageUrl = ''
      }

      this.selectedFile = e.target.files
      if(this.selectedFile.length){
        console.log("Uploading file to Imgur..");

        var apiUrl = 'https://api.imgur.com/3/image';
        var apiKey = '36757703a6732ed';

        var settings = {
          async: false,
          crossDomain: true,
          processData: false,
          contentType: false,
          type: 'POST',
          url: apiUrl,
          headers: {
            Authorization: 'Client-ID ' + apiKey,
            Accept: 'application/json'
          },
          mimeType: 'multipart/form-data'
        };

        var formData = new FormData();
        formData.append("image", this.selectedFile[0]);
        settings.data = formData;


        $.ajax(settings).done(response => {
          this.img = JSON.parse(response).data.link;
        });
      }
    },


    onFileSelected(event){
      this.checkRandom=false;
      this.selectedFile = event.target.files
      if(this.selectedFile.length){
        console.log("Uploading file to Imgur..");

        var apiUrl = 'https://api.imgur.com/3/image';
        var apiKey = '36757703a6732ed';

        var settings = {
          async: false,
          crossDomain: true,
          processData: false,
          contentType: false,
          type: 'POST',
          url: apiUrl,
          headers: {
            Authorization: 'Client-ID ' + apiKey,
            Accept: 'application/json'
          },
          mimeType: 'multipart/form-data'
        };

        var formData = new FormData();
        formData.append("image", this.selectedFile[0]);
        settings.data = formData;


        $.ajax(settings).done(response => {
          this.img = JSON.parse(response).data.link;
        });
      }

    }
  }
}
</script>

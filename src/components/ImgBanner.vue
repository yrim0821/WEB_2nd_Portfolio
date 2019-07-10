<template>
  <div>
    <v-img :src="imgSrc"
           aspect-ratio="1.7">
      <v-layout align-center justify-center row fill-height>
        <v-flex text-xs-center>
          <span class="text-shadow display-2 font-weight-light">
            <slot name="text"/>
          </span>
        </v-flex>
      </v-layout>
    </v-img>
    <form id="imgur">
      <input type="file" @change="onFileSelected" class="imgur" accept="image/*" data-max-size="5000"/>
    </form>
  </div>
</template>

<script src="https://cdn.jsdelivr.net/npm/jquery@3.2.1/dist/jquery.min.js"></script>
<script>

import FirebaseService from '@/services/FirebaseService'
import firebase from 'firebase/app'
import $ from 'jquery'
export default {
  name: 'App',
   name: 'ImgBanner',
   data () {
     return{
       selectedFile: null,
       imgSrc:''
     }
   },
   props: {
      text: {type: String},
      state: {type: String}
   },
   methods: {
     onFileSelected(event){
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
           var str = response.split(",");
           var getimgurl = str[27].split("\"");
           console.log(getimgurl[3]);
           this.$store.state.imgSrc = getimgurl[3];
           console.log(this.$store.state.imgSrc);
           this.imgSrc = this.$store.state.imgSrc;
           FirebaseService.postBanner(this.imgSrc);
         });
       }

     }
   },
   mounted(){
     this.imgSrc = FirebaseService.getBanner();
   }
}

</script>

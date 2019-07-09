<template>
  <div>
    <ImgBanner imgSrc="https://source.unsplash.com/random">
      <div style="line-height:1.2em;" slot="text">Just Do IT.</div>
    </ImgBanner>
    <v-container>
      <!-- About Me -->
      <v-layout my-5>
        <v-flex class="aboutMe" :class="$mq" xs8>
          <h2 class="headline mb-3">About Me</h2>
          <p class="mr-4" v-line-clamp:20="5">안녕하세요!
SSAFY 1기 석주연입니다.
경북대학교 전자공학과를 졸업해서 웹 개발에 대해 배운지 얼마안됐지만,
부족한 점이 많지만 차근차근 배워서 한 사람의 몫 이상을 해내는 개발자가 되고싶습니다.
잘 부탁드립니다 : )</p>
          </v-flex>
          <v-flex class="profileImg" :class="$mq" xs4>
            <v-img :src="getImgUrl('profile.png')" aspect-ratio="1.5" />
          </v-flex>
        </v-layout>

        <!--clock-->
        <v-layout my-5>
          <v-flex xs12>
            <clock/>
          </v-flex>
        </v-layout>

        <!--weather-->
        <v-layout my-5>
          <v-flex xs12>
            <a class="weatherwidget-io" href="https://forecast7.com/en/36d12128d34/gumi-si/" data-label_1="GUMI" data-label_2="WEATHER" data-theme="weather_one" >GUMI WEATHER</a>
          </v-flex>
        </v-layout>

        <!-- Portfolio -->
        <v-layout my-5>
          <v-flex xs12>
            <router-link to="/portfolio">
              <h2 class="headline my-5 text-xs-center">Portfolio</h2>
              <PortfolioList :column="$mq==='mobile'?1:2"></PortfolioList>
            </router-link>
          </v-flex>
        </v-layout>

        <!-- Post -->
        <v-layout my-5>
          <v-flex xs12>
            <router-link to="/post"><h2 class="headline my-5 text-xs-center">Post</h2></router-link>
            <PostList :column="$mq==='mobile'?1:2"></PostList>
          </v-flex>
        </v-layout>


        <!-- Github -->
        <v-layout my-5>
          <v-flex xs12>
            <h2 class="headline my-5 text-xs-center">Project</h2>
            <RepositoryList></RepositoryList>
          </v-flex>
        </v-layout>
      </v-container>
    </div>
  </template>
  <script>
  import ImgBanner from '../components/ImgBanner'
  import PortfolioList from '../components/PortfolioList'
  import PostList from '../components/PostList'
  import RepositoryList from '../components/RepositoryList'
  import clock from '../components/clock'
  import axios from 'axios';
  export default {
    name: 'HomePage',
    components: {
      ImgBanner,
      PortfolioList,
      PostList,
      RepositoryList,
      clock
    },
    created(){
      axios.get('http://localhost:8080')
      .then(response=>(this.info=response))
      .catch()
    },
    data(){
      return{
        info:null
      }
    },
    methods: {
      getImgUrl(img) {
        return require('../assets/' + img)
      }
    }
  }



  !function(d,s,id){
    var js,fjs=d.getElementsByTagName(s)[0];
    if(!d.getElementById(id)){
      js=d.createElement(s);
      js.id=id;
      js.src='https://weatherwidget.io/js/widget.min.js';
      fjs.parentNode.insertBefore(js,fjs);
    }
  }(document,'script','weatherwidget-io-js');

  </script>

  <style>
  .profileImg.mobile{
    display: none;
  }
  .aboutMe.mobile{
    text-align: center;
    margin: 0 auto;
    font-size: 2vw;
  }

  .aboutMe.mr-4{
    overflow-y: scroll;
  }
  </style>

<template>

  <div>
    <div style="margin-top:50px"></div>
    <ImgBanner>
      <div id="BannerTitle" style="line-height:1.2em;" slot="text">Even Dead I'm The Hero.</div>
    </ImgBanner>
    <v-container>
      <!-- About Me -->
      <v-layout my-5>
        <v-flex class="aboutMe" :class="$mq" xs8>
          <h2 class="headline mb-3">About Us</h2>
          <p class="mr-4" v-line-clamp:20="5">안녕하세요!
            구미 통합 1반 6조입니다. 우리는 다양한 전공자들로 구성되어 있습니다.<br>
            다양한 전공자들이 모여 그룹을 형성하기란 쉽지 않은 일입니다.<br>
            우리 조의 콘셉트는 서로가 서로를 포용하며 조화를 이루는 것입니다.</p>
          </v-flex>
          <v-flex class="profileImg" :class="$mq" xs4>
            <v-img :src="getImgUrl('harmony.png')" aspect-ratio="1.5" />
          </v-flex>
        </v-layout>

        <!--clock-->
        <v-layout my-5>
          <v-flex xs12>
            <clock/>
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
  import firebase from 'firebase/app'

  export default {
    name: 'HomePage',
    components: {
      ImgBanner,
      PortfolioList,
      PostList,
      RepositoryList,
      clock
    },
    methods: {
      getImgUrl(img) {
        return require('../assets/team6/logo/' + img)
      },
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
  #BannerTitle{
    z-index: 1;
    background: linear-gradient(90deg, rgba(240,20,30,5) 0%, rgba(150,120,220,1) 46%, rgba(250,240,0,5) 100%);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
  }
  </style>

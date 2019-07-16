<template>
  <div>
    <div style="margin-top:48px"></div>
    <div class="notranslate">
      <ImgBanner>
        <div id="bannerTitle" style="line-height:1.2em; font-size:7rem;" slot="text">Orchestrate Life.</div>
      </ImgBanner>
    </div>
    <v-container>
      <!-- About Me -->
      <v-layout my-5>
        <v-flex class="aboutMe" :class="$mq" xs8>
          <p class="headline mb-3">About Us

            <v-btn v-if="this.$store.state.langage==0" flat icon @click='refreshKR()' href="#googtrans(en|ko)"><img src="img/KR.png" alt="KOREA" width="25px"></v-btn>
            <v-btn v-else flat icon @click='refreshUS()' href="#googtrans(en|en)"><img src="img/US.png" alt="USA" width="25px"></v-btn>

          <p class="mr-4" v-line-clamp:20="5">안녕하세요!
            구미 통합 1반 6조입니다. 우리는 다양한 전공자들로 구성되어 있습니다.<br>
            다양한 전공자들이 모여 그룹을 형성하기란 쉽지 않은 일입니다.<br>
            우리 조의 콘셉트는 서로가 서로를 포용하며 조화를 이루는 것입니다.
          </p>
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
            <h2 class="headline my-5 text-xs-center notranslate">Portfolio</h2>
            <PortfolioList :column="$mq==='mobile'?1:2"></PortfolioList>
          </router-link>
        </v-flex>
      </v-layout>

      <!-- Post -->
      <v-layout my-5>
        <v-flex xs12>
          <router-link to="/post"><h2 class="headline my-5 text-xs-center notranslate">Post</h2></router-link>
          <PostList :column="$mq==='mobile'?1:2"></PostList>
        </v-flex>
      </v-layout>

      <!-- Github -->
      <v-layout my-5>
        <v-flex xs12>
          <h2 class="headline my-5 text-xs-center notranslate"><span style="font-size:25pt;">Project</span>
          </h2>
          <v-layout align-center>
            <v-flex xs6 grow>
              <RepositoryList class="notranslate"></RepositoryList>
            </v-flex>
            <v-flex xs6 shrink class="show-on-scroll">
              <membersgraph></membersgraph>
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>

      <!-- Graph -->
      <div class='notranslate'>
        <v-layout my-5 v-show="$mq==='mobile' ? false : true">
          <v-flex xs12>
            <h2 class="headline my-5 text-xs-center notranslate"><span style="font-family:'Jeju Hallasan'; font-size:18pt;">Graph</span>
            </h2>
            <gitgraph></gitgraph>
          </v-flex>
        </v-layout>
      </div>
    </v-container>
  </div>
</template>
<script>
import membersgraph from '../components/membersgraph'
import ImgBanner from '../components/ImgBanner'
import PortfolioList from '../components/PortfolioList'
import PostList from '../components/PostList'
import RepositoryList from '../components/RepositoryList'
import clock from '../components/clock'
import firebase from 'firebase/app'
import gitgraph from '../components/gitgraph'

export default {
  name: 'HomePage',
  components: {
    ImgBanner,
    PortfolioList,
    PostList,
    RepositoryList,
    clock,
    gitgraph,
    membersgraph
  },
  data(){
    return{
      toggle_one : localStorage.getItem('langage')
    }
  },
  methods: {
    refreshKR(){
      localStorage.setItem('langage', 1)
      setTimeout((function() {
        window.location.reload();
      }), 250);
    },
    refreshUS(){
      localStorage.setItem('langage', 0)
      setTimeout((function() {
        window.location.reload();
      }), 250);
    },
    getImgUrl(img) {
      return require('../assets/team6/logo/' + img)
    },
  },
  mounted(){
    this.$store.state.langage = localStorage.getItem('langage');
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

.headline.my-5.text-xs-center{
  margin: auto;
  color:#1AAB8A;
  border:none;
  position:relative;
  width: 30%;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
  padding: 15px 0px;
}

.headline.my-5.text-xs-center:hover{
  background:#1AAB8A;
  color:#fff;
}
.headline.my-5.text-xs-center:before,.headline.my-5.text-xs-center:after{
  content:'';
  position:absolute;
  top:0;
  right:0;
  height:2px;
  width:100%;
  background: #1AAB8A;
  transition:400ms ease all;

}
.headline.my-5.text-xs-center:after{
  right:inherit;
  top:inherit;
  left:0;
  bottom:0;
  background:#1AAB8A;
}
.headline.my-5.text-xs-center:hover:before,.headline.my-5.text-xs-center:hover:after{
  width:0;
  transition:800ms ease all;
}
</style>

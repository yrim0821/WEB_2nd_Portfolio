<template>
  <v-app>
    <MainHeader/>
    <v-content>
      <router-view/>
    </v-content>
    <back-to-top bottom="20px" right="10px">
      <v-btn flat icon type="button" class="btn btn-info btn-to-top notranslate"><v-icon>keyboard_arrow_up</v-icon></v-btn>
    </back-to-top>
    <MainFooter/>
  </v-app>
</template>

<script>
import store from './store'
import MainHeader from './components/MainHeader'
import MainFooter from './components/MainFooter'
import Vue from 'vue'
import BackToTop from 'vue-backtotop'
Vue.use(BackToTop)

var agent = navigator.userAgent.toLowerCase();
var name = navigator.appName;

// MS 계열 브라우저를 구분  IE 11+, IE 11,Edge
if(name === 'Microsoft Internet Explorer' || agent.indexOf('trident') > -1 || agent.indexOf('edge/') > -1) {
  alert("해당 브라우저는 크롬에 최적화 되어 있습니다.");
} else if(agent.indexOf('safari') > -1) { // Chrome or Safari
  if(agent.indexOf('chrome') > -1) { // Chrome
  }else{
    alert("해당 브라우저는 크롬에 최적화 되어 있습니다.");
  }
} else if(agent.indexOf('firefox') > -1) { // Firefox
  alert("해당 브라우저는 크롬에 최적화 되어 있습니다.");
}

export default {
  name: 'App',
  store,
  data() {
    return {
      //
    }
  },
  components: {
    'MainHeader' : MainHeader,
    'MainFooter' : MainFooter,
    BackToTop
  }
}
</script>
<style>
@font-face {
  font-family: 'Swagger';
  src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_two@1.0/Swagger.woff') format('woff');
  font-weight: normal;
  font-style: normal;
  }
*{
  font-family: Swagger;
}
#bannerTitle{
  z-index: 1;
  background: linear-gradient(90deg, rgba(240,20,30,5) 0%, rgba(150,120,220,1) 46%, rgba(250,240,0,5) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>

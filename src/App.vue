<template>
  <v-app>
    <MainHeader/>
    <v-content>
      <router-view/>
    </v-content>
    <back-to-top bottom="50px" right="50px">
      <button type="button" class="btn btn-info btn-to-top"><i class="fa fa-chevron-up"></i></button>
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
.btn-to-top {
  width: 60px;
  height: 60px;
  padding: 10px 16px;
  border-radius: 50%;
  font-size: 22px;
  line-height: 22px;
}
@font-face {
  font-family: 'Swagger';
  src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_two@1.0/Swagger.woff') format('woff');
  font-weight: normal;
  font-style: normal;
  }
*{
  font-family: Swagger;
}
</style>

<template>
  <v-app style="position:absolute">
    <div class="toolbar">
      <v-toolbar id="header">
        <v-toolbar-side-icon
        @click.stop="sideNav = !sideNav"
        class="hidden-sm-and-up"><v-icon>menu</v-icon>
      </v-toolbar-side-icon>
      <v-toolbar-title>
        <router-link to="/"><v-img id="breadth_logo" :src="getImgUrl('breadth_logo.png')"/></router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <div id="google_translate_element"></div>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn flat><router-link to="/portfolio">Portfolio</router-link></v-btn>
        <v-btn flat><router-link to="/post">Post</router-link></v-btn>

        <v-btn v-if="!$store.state.user" flat><router-link to="/login">login</router-link></v-btn>
        <v-btn v-else flat v-on:click="logout"><router-link to="/login">logout</router-link></v-btn>

<v-btn v-on:click="bookmarksite('ujj', 'http://ujj.com')"><v-icon>bookmark</v-icon></v-btn>
      </v-toolbar-items>
    </v-toolbar>

  </div>

  <v-navigation-drawer temporary style="position:fixed" v-model="sideNav">
    <v-list>
      <v-list-tile
      v-for="item in menuItems"
      :key="item.title"
      :to="item.link">
      <v-list-tile-action>
        <v-icon>{{ item.icon }}</v-icon>
      </v-list-tile-action>
      <v-list-tile-content>{{ item.title }}</v-list-tile-content>
    </v-list-tile>


    <v-list-tile v-if="!$store.state.user" to="/login">
      <v-list-tile-action>
        <v-icon>exit_to_app</v-icon>
      </v-list-tile-action>
      <v-list-tile-content>Login</v-list-tile-content>
    </v-list-tile>

    <v-list-tile v-else v-on:click="logout">
      <v-list-tile-action>
        <v-icon>exit_to_app</v-icon>
      </v-list-tile-action>
      <v-list-tile-content>Logout</v-list-tile-content>
    </v-list-tile>


  </v-list>
</v-navigation-drawer>
</v-app>

</template>

<script>
import firebase from 'firebase/app'

export default {
  name: 'MainHeader',
  data () {
    return{
      sideNav: false,
      user: firebase.auth().currentUser,
      menuItems: [
        { icon: 'home', title: 'home', link: '/' },
        { icon: 'portrait', title: 'Portfolio', link: '/portfolio' },
        { icon: 'photo_filter', title: 'Post', link: '/post' }
      ]
    }
  },
  methods: {
    getImgUrl(img) {
      return require('../assets/team6/logo/' + img)
    },
    bookmarksite(title, url) {
      var agent = navigator.userAgent.toLowerCase();
      var name = navigator.appName;

    // MS 계열 브라우저를 구분  IE 11+, IE 11,Edge
      if(name === 'Microsoft Internet Explorer' || agent.indexOf('trident') > -1 || agent.indexOf('edge/') > -1) {
        window.external.AddFavorite(url, title);
      }
      else if(agent.indexOf('safari') > -1) { // Chrome or Safari
        if(agent.indexOf('chrome') > -1) { // Chrome
          alert("Ctrl+D키를 누르시면 즐겨찾기에 추가하실 수 있습니다.");
        }else{
          alert("Ctrl+D키를 누르시면 즐겨찾기에 추가하실 수 있습니다.");
        }
      }
      else if(agent.indexOf('firefox') > -1) { // Firefox
        alert("Ctrl+D키를 누르시면 즐겨찾기에 추가하실 수 있습니다.");
        window.sidebar.addPanel(title, url, " ");
      }
      else if (window.opera && window.print) { // opera
        var elem = document.createElement('a');
        elem.setAttribute('href', url);
        elem.setAttribute('title', title);
        elem.setAttribute('rel', 'sidebar');
        elem.click();
      }
    },
    logout(){
      if(firebase.auth().currentUser == null){
        alert("로그인 후 이용해주세요.");
      }
      else if(firebase.auth().currentUser){
        firebase.auth().signOut().then(() => {
          if(this.$store.state.user.displayName == null){
            alert(this.$store.state.user + "님 로그아웃 되었습니다.");
          }else{
            alert(this.$store.state.user.displayName + "님 로그아웃 되었습니다.");
          }
          this.$store.state.accessToken = '';
          this.$store.state.user = '';
        }).catch(function(error) {
          alert(error);
        });
      }
    }
  },
  mounted(){

  }
}
</script>
<style>
#breadth_logo{
  width: 140px;
}
#header{
  position: fixed;
  z-index: 6;
  background-color: white;
  width:100%;
}
a{
  text-decoration: none;
  font-size: 20px;
}
</style>

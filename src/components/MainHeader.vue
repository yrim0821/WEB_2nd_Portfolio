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

      <!-- 번역 버튼 -->
       <v-btn flat @click='refresh()' href="#googtrans(en|en)" class="lang-en lang-select" data-lang="en"><img src="img/US.png" alt="USA" height="30px" width="45px"></v-btn>
       <v-btn flat @click='refresh()' href="#googtrans(en|ko)" class="lang-es lang-select" data-lang="ko"><img src="img/KR.png" alt="KOREA" height="30px" width="45px"></v-btn>

      <v-toolbar-items class="hidden-xs-only">
        <v-btn flat><router-link to="/portfolio">Portfolio</router-link></v-btn>
        <v-btn flat><router-link to="/post">Post</router-link></v-btn>


          <v-dialog v-if="!$store.state.user" v-model="loginDialog" width="380">
            <template v-slot:activator="{ on }">
              <v-btn flat v-on="on"><router-link to="">login</router-link></v-btn>
            </template>

            <v-card>
              <v-img :src="getImgUrl('login_form.png')" style="width:100%">

              <v-card-text style="margin-top:150px">
                <div style="padding-left:50px;">
                  <v-text-field v-model="loginEmail" label="Email" placeholder="Placeholder" style="width:250px;"></v-text-field>
                  <v-text-field v-model="loginPassword" label="Password" placeholder="Placeholder" type="password" style="width:250px;"></v-text-field>


                  <v-btn round color="#20aa49" dark v-on:click="loginWithMail" style="width:230px;;height:25px;"><v-icon size="20" class="mr-2">mail</v-icon> Mail 로그인</v-btn>
                  <v-btn round color="#df4a31" dark v-on:click="loginWithGoogle" style="width:230px;;height:25px;"><v-icon size="20" class="mr-2">fa-google</v-icon> Google 로그인</v-btn>
                  <v-btn round color="#4267B2" dark v-on:click="loginWithFacebook" style="width:230px;;height:25px;"><v-icon size="20" class="mr-2">fa-facebook</v-icon> Google 로그인</v-btn>

                  <v-dialog v-model="signupDialog" width="500">
                    <template v-slot:activator="{ on }">
                      <v-btn round color="#888888" dark v-on="on" style="width:230px;height:25px;"><v-icon size="20" class="mr-2">person</v-icon>회원가입</v-btn>
                    </template>

                    <v-card>
                      <v-card-title class="headline grey lighten-2" primary-title>Sign up</v-card-title>
                      <v-card-text>
                        <form>
                          <v-text-field v-model="signupEmail" label="Email" placeholder="Placeholder"></v-text-field>
                          <v-text-field v-model="signupPassword" label="Password" placeholder="Placeholder"></v-text-field>
                        </form>
                      </v-card-text>
                      <v-divider></v-divider>

                      <v-card-actions>
                        <v-spacer></v-spacer>
                          <v-btn color="primary" flat v-on:click="signUp()">SignUp</v-btn>
                          <v-btn color="primary" flat @click="signupDialog = false">close</v-btn>
                      </v-card-actions>
                    </v-card>

                  </v-dialog>
                </div>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                  <v-btn color="primary" flat @click="loginDialog = false">close</v-btn>
              </v-card-actions>
            </v-img>
            </v-card>

          </v-dialog>

        <v-btn v-else flat v-on:click="logout"><router-link to="/login">logout</router-link></v-btn>
        <v-btn v-on:click="bookmarksite('ujj', 'http://ujj.com')"><v-icon class='notranslate'>bookmark</v-icon></v-btn>
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
import FirebaseService from '@/services/FirebaseService'
import firebase from 'firebase/app'

export default {
  name: 'MainHeader',
  data () {
    return{
      loginEmail: '',
      loginPassword: '',
      signupEmail: '',
      signupPassword: '',
      loginDialog: false,
      signupDialog: false,
      sideNav: false,
      user: '',
      accessToken: '',
      menuItems: [
        { icon: 'home', title: 'home', link: '/' },
        { icon: 'portrait', title: 'Portfolio', link: '/portfolio' },
        { icon: 'photo_filter', title: 'Post', link: '/post' }
      ]
    }
  },
  methods: {
    // 구글번역
    refresh(){
      setTimeout((function() {
        window.location.reload();
      }), 250);
    },
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
    async loginWithMail(){
      firebase.auth().signInWithEmailAndPassword(this.loginEmail, this.loginPassword)
      .then((user)=>{
        this.$store.state.accessToken = user.user.refreshToken
        this.$store.state.user = user.user
        localStorage.setItem('accessToken', this.$store.state.accessToken)
        localStorage.setItem('user', JSON.stringify(user.user))
        alert(this.$store.state.user.email + "님 로그인 되었습니다")
        this.$router.replace('/')
      })
      .catch((error)=>{
        alert(error)
      })
    },
    async loginWithGoogle() {
       const result = await FirebaseService.loginWithGoogle()
       this.$store.state.accessToken = result.credential.accessToken
       this.$store.state.user = result.user
       localStorage.setItem('accessToken', this.$store.state.accessToken)
       localStorage.setItem('user', JSON.stringify(result.user))
       alert(this.$store.state.user.email + "님 로그인 되었습니다")
      this.$router.replace('/')
    },
    async loginWithFacebook() {
         const result = await FirebaseService.loginWithFacebook()
         this.$store.state.accessToken = result.credential.accessToken
         this.$store.state.user = result.user
         localStorage.setItem('accessToken', this.$store.state.accessToken)
         localStorage.setItem('user', JSON.stringify(result.user))
      alert(this.$store.state.user.displayName + "님 로그인 되었습니다")
      this.$router.replace('/')
      },
    signUp(){
      firebase.auth().createUserWithEmailAndPassword(this.signupEmail, this.signupPassword)
      .then((user)=>{
        alert("Your account has been created!")
        this.loginDialog = false;
        this.signupDialog = false;
        this.$router.replace('/')
      })
      .catch((error)=>{
        alert(error)
      })
    },
    logout(){
      if(firebase.auth().currentUser == null){
        alert("로그인 후 이용해주세요.");
        this.$store.state.accessToken = '';
        this.$store.state.user = '';
        localStorage.setItem('accessToken', this.$store.state.accessToken)
        localStorage.setItem('user', this.$store.state.user)
        this.$router.replace('/')
      }
      else if(firebase.auth().currentUser){
        firebase.auth().signOut().then(() => {
          alert(this.$store.state.user.email + "님 로그아웃 되었습니다.");
          this.$store.state.accessToken = '';
          this.$store.state.user = '';
          localStorage.setItem('accessToken', this.$store.state.accessToken)
          localStorage.setItem('user', this.$store.state.user)
          this.loginDialog = false;
          this.signupDialog = false;
          this.$router.replace('/')
        }).catch(function(error) {
          alert(error);
        });
      }

    }
  },
  mounted(){
    if(localStorage.getItem("user")==null || localStorage.getItem("user")==''){

    }else{
      this.$store.state.user = JSON.parse(localStorage.getItem("user") || "{}");
      this.$store.state.accessToken = localStorage.getItem('accessToken');
    }

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

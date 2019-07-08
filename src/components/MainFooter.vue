<template>
  <v-footer height="auto" color="indigo" dark>
          <v-layout justify-center row wrap>
            <!--weather-->
            <v-layout>
              <v-flex xs12>
                <a class="weatherwidget-io" href="https://forecast7.com/en/36d12128d34/gumi-si/" data-label_1="GUMI" data-label_2="WEATHER" data-theme="weather_one" >GUMI WEATHER</a>
              </v-flex>
            </v-layout>
            <v-flex color="indigo" dark py-3 text-xs-center white--text xs12>
              &copy;2019 — <strong>Vuetify</strong>
            </v-flex>
          </v-layout>
        </v-footer>
</template>

<script>
export default {
  data: () => ({
    drawer: null
  })
};
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

<style scoped>
</style>

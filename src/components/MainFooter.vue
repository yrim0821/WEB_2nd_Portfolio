<template>
  <div class="notranslate">
    <v-footer dark id="footcolor" height="auto" color="indigo">
      <v-layout justify-center row wrap>
        <!--weather-->
        <v-layout column style="z-index: 2;  opacity: 0.8;">
          <v-flex xs12>
            <a id="footcolor" class="weatherwidget-io" href="https://forecast7.com/en/36d12128d34/gumi-si/" data-label_1="GUMI" data-label_2="WEATHER" data-theme="weather_one" >GUMI WEATHER</a>
          </v-flex>

          <v-flex xs12 text-xs-right>
            @provided by 맑은유림조 @2019.07.10 @git:lab.ssafy.com
          </v-flex>

        </v-layout>
    </v-layout>
  </v-footer>
</div>
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
#footcolor{
  z-index: 1;
  background: rgb(18,194,233);
  background: linear-gradient(90deg, rgba(18,194,233,1) 0%, rgba(196,113,237,1) 46%, rgba(246,79,89,1) 100%);
}
</style>

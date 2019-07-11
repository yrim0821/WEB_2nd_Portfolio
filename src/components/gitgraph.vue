
<template>
  <div class="notranslate" style="margin-top: 200px;">
    <div class="calendarContainer">
      <div class="calendarBox" id="calendar_basic" style="width: 1000px; height: 350px;"></div>
    </div>
    <v-btn v-on:click="getCommits('6097')">프로젝트조회</v-btn>
    <v-btn v-on:click="getRepos('myccpb08')">나</v-btn>
    <v-btn v-on:click="getRepos('Kim_yh')">영훈</v-btn>
    <v-btn v-on:click="getRepos('JIGyeongmin')">경민</v-btn>
    <v-btn v-on:click="getRepos('LeeSuKyeong')">수경</v-btn>
    <v-btn v-on:click="getRepos('seok')">주연</v-btn>
  </div>
</template>

<script>
// var CORS = require('cors')();
// app.use(CORS);

const BASE_URL = "https://lab.ssafy.com/api/v4";
google.charts.load("current", { packages: ["calendar"] });
google.charts.setOnLoadCallback(drawChart);

$(function() {
  drawChart();
});

// https://developers.google.com/chart/interactive/docs/gallery/calendar
function drawChart(datas, testing) {
  // 힝힝 //


  //
  var dataTable = new google.visualization.DataTable();  // 본문 그대로

  let date = new Date(); // gitservice.js 에 있는 그거 commit 불러 오려고
  let timeline = [];
  let year = date.getFullYear();
  let month = date.getMonth();
  let day = date.getDay();
  let personal_title = "Gitlab Graph"

  if(datas != undefined) {
    for(let index = 0; datas[index] != null; index++) {
      // 2019-07-09T10:19:42.209+09:00
      timeline.push(new Array(new Date(new Date(datas[index].created_at).getFullYear(), new Date(datas[index].created_at).getMonth(), new Date(datas[index].created_at).getDay()), index));
      //new Date(new Date(datas[index].created_at).getFullYear(), new Date(datas[index].created_at).getMonth(), new Date(datas[index].created_at).getDay()), index)
      //[Wed Jul 03 2019 00:00:00 GMT+0900 (한국 표준시), 1]
    }
    personal_title = datas[0].owner.name + " " + datas[0].owner.username;
  }
  //   else
  //     timeline.push(new Array(new Date(0, 0, 0), 100));

  // docs 그대로 복붙
  dataTable.addColumn({ type: "date", id: "Date" });
  dataTable.addColumn({ type: "number", id: "Won/Loss" });
  dataTable.addRows(timeline);

  var chart = new google.visualization.Calendar(
    document.getElementById("calendar_basic")
  );

  var options = {
    title: personal_title,
    height: 500,
    colorAxis: {
      maxValue: 100,
      minValue: 0
    }
  };

  chart.draw(dataTable, options);
}
// ↑ docs 복붙

export default {
  data() {
    return {
      ret: "",
      ret2: ""
    };
  },
  methods: {
    getRepos(id) {
      fetch(
        `${BASE_URL}/users/${id}/projects?private_token=5yRamVkqs4Z4bq-G1roY`
      )

      .then(res => {
        return res.json();
      })


      .then(data => {
        this.ret2 = data;
        drawChart(this.ret2, "테스팅");
      });
    },
    getCommits(id) {
      fetch(
        `${BASE_URL}/projects/${id}/repository/commits?private_token=5yRamVkqs4Z4bq-G1roY`
      )
      .then(res => {
        return res.json();
      })
      .then(data => {
        this.ret = data;
      });
    }
  }
};
</script>

<style>

.calendarContainer {

  position: relative;
  width: 1000px;
  height: 350px;
}

.calendarBox {

  position: absolute;
  top: 10%;
  left: 1%;
  margin: auto;
}

</style>

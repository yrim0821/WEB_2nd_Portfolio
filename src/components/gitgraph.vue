<template>
<div style="margin-top: 200px;">
  <div class="calendarContainer">
    <div class="calendarBox" id="calendar_basic" style="width: 1000px; height: 350px;"></div>
  </div>
  <v-btn v-on:click="getCommits()">프로젝트</v-btn>
  <v-btn v-on:click="getRepos('myccpb08','5yRamVkqs4Z4bq-G1roY')">나</v-btn>
  <v-btn v-on:click="getRepos('Kim_yh', 'N9RKhWdxvbGzn3oYEwVe')">영훈</v-btn>
  <v-btn v-on:click="getRepos('JIGyeongmin', 'yYcb5LEDsxxbN1PPxKEj')">경민</v-btn>
  <v-btn v-on:click="getRepos('LeeSuKyeong','dCp7MpuwFQNzYrLBZix5')">수경</v-btn>
  <v-btn v-on:click="getRepos('seok','xTftb51x12NTwFbxxAC5')">주연</v-btn>
</div>
</template>

<script>

const BASE_URL = "https://lab.ssafy.com/api/v4";
google.charts.load("current", {
  packages: ["calendar"]
});
google.charts.setOnLoadCallback(drawChart);

$(function() {
  drawChart();
});

// https://developers.google.com/chart/interactive/docs/gallery/calendar
function drawChart(datas) {
  let personal_title = "그래프"
  var dataTable = new google.visualization.DataTable();
  var timeline = [];
  var cnt = [];
  var info = [];

  if (datas != undefined) { // datas 로 반복문 돌림
    for (let index = 0; datas[index] != null; index++) {
      var data = datas[index].created_at;
      if(data == null) continue;

      var year = new Date(data).getFullYear()
      var month = new Date(data).getMonth() + 1
      var day = new Date(data).getDate()
      var temp = year + ',' + month + ',' + day

      var cccnt = 1
      var len = timeline.length
      for (var idx = 0; idx <= len; idx++) {
        if (idx == len) {
          timeline.push(temp)
          cnt.push(cccnt)
          break
        } else if (timeline[idx] == temp) {
          cnt[idx]++
          break
        }
      }
    }  // for문 끝

    var a = timeline.length
    for (let idx = 0; idx<a; idx++){
      var temp = [new Date(timeline[idx]), cnt[idx]]
      info.push(temp)
    }

    dataTable.addColumn({ type: "date", id: "Date" });
    dataTable.addColumn({ type: "number", id: "Won/Loss" });
    dataTable.addRows(info);
    // console.log(timeline)

    var chart = new google.visualization.Calendar(
      document.getElementById("calendar_basic")
    );

    var options = {
      title: personal_title,
      height: 500,
      colorAxis: {
          maxValue: 20,
          minValue: 0,
          colors:['white','orange'],
      },
      calendar:{
        monthOutlineColor: {
          stroke: '#D8AF91',
          strokeOpacity: 0.5,
          strokeWidth: 1
        },
      },

    };

    chart.draw(dataTable, options);
  }
} // draw chart 끝

export default {
  data() {
    return {
      ret: "",
      ret2: ""
    };
  },
  methods: {
    getRepos(id, token) {
      fetch(
          `${BASE_URL}/users/${id}/events?namespaces&per_page=100&private_token=${token}`
        )

        .then(res => {
          return res.json();
        })


        .then(data => {
          //this.ret2 = data;  // data = contribution 들의 집합
          drawChart(data);
        });
    },
    getCommits(id) {
      fetch(
        `${BASE_URL}/projects/6097/repository/commits??namespaces&per_page=100&private_token=5yRamVkqs4Z4bq-G1roY`
      )
        .then(res => {
          return res.json();
        })
        .then(data => {
          console.log(data)
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

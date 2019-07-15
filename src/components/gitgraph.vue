<template>
  <div style="margin-top: 50px;">
    <div class="calendarContainer">
      <div class="calendarBox" id="calendar_basic" style="width: 800px; height: 250px;"></div>
    </div>
    <v-btn fab outline large color="cyan" v-on:click="getCommits()">프로젝트</v-btn>
    <v-btn fab outline color="success" v-on:click="getRepos('myccpb08','5yRamVkqs4Z4bq-G1roY')">유림</v-btn>
    <v-btn fab outline color="success" v-on:click="getRepos('Kim_yh', 'N9RKhWdxvbGzn3oYEwVe')">영훈</v-btn>
    <v-btn fab outline color="success" v-on:click="getRepos('JIGyeongmin', 'yYcb5LEDsxxbN1PPxKEj')">경민</v-btn>
    <v-btn fab outline color="success" v-on:click="getRepos('LeeSuKyeong','dCp7MpuwFQNzYrLBZix5')">수경</v-btn>
    <v-btn fab outline color="success" v-on:click="getRepos('seok','xTftb51x12NTwFbxxAC5')">주연</v-btn>
  </div>
</template>

<script>
const BASE_URL = "https://lab.ssafy.com/api/v4";
google.charts.load("current", {
  packages: ["calendar"]
});
google.charts.setOnLoadCallback(drawChart);

// https://developers.google.com/chart/interactive/docs/gallery/calendar
function drawChart(datas) {
  let personal_title =  datas[0].author.name + " " + datas[0].author.username ;
  var dataTable = new google.visualization.DataTable();
  var timeline = [];
  var cnt = [];
  var info = [];

  if (datas != undefined) {
    // datas 로 반복문 돌림
    for (let index = 0; datas[index] != null; index++) {
      var data = datas[index].created_at;
      if (data == null) continue;

      var year = new Date(data).getFullYear();
      var month = new Date(data).getMonth() + 1;
      var day = new Date(data).getDate();
      var temp = year + "," + month + "," + day;

      var cccnt = 1;
      var len = timeline.length;
      for (var idx = 0; idx <= len; idx++) {
        if (idx == len) {
          timeline.push(temp);
          cnt.push(cccnt);
          break;
        } else if (timeline[idx] == temp) {
          cnt[idx]++;
          break;
        }
      }
    } // for문 끝

    var a = timeline.length;
    for (let idx = 0; idx < a; idx++) {
      var temp = [new Date(timeline[idx]), cnt[idx]];
      info.push(temp);
    }

    dataTable.addColumn({ type: "date", id: "Date" });
    dataTable.addColumn({ type: "number", id: "Won/Loss" });
    dataTable.addRows(info);

    var chart = new google.visualization.Calendar(
      document.getElementById("calendar_basic")
    );

    var options = {
      title: personal_title,
      height: 300,
      colorAxis: {
        maxValue: 30,
        minValue: 0,
        colors: ["#faed7d", "ff5e00"]
      },
      calendar: {
        underMonthSpace: 10,

        monthLabel: {
        fontName : 'BBTreeGR',
        fontSize: 12,
        color: '#8C8C8C',
      },
        monthOutlineColor: {
          stroke: "#D8AF91",
          strokeOpacity: 0.5,
          strokeWidth: 1
        },
        cellSize: 13,

        dayOfWeekLabel:{
          fontName : 'BBTreeGR',
          fontSize:8,
          color: 'black',
          bold: true,
          italic: true,
          color:  '#8C8C8C',
        },
        dayOfWeekRightSpace: 10,
      },
      noDataPattern : {
        backgroundColor : '#EAEAEA', // 줄무늬
        color : '#F6F6F6'
      },
    };

    chart.draw(dataTable, options);
  }
} // draw chart 끝

/////////////////////////////////////

export default {
  data() {
    return {
    };
  },
  methods: {
    getRepos(id, token) {
      var request = require("request");
      var headers = {
        "PRIVATE-TOKEN": token
      };
      var options = {
        url: "https://lab.ssafy.com/api/v4/users/" + id + "/events",
        headers: headers
      };

      function callback(error, response, body) {
        if (!error && response.statusCode == 200) {
          var howmany = response.headers["x-total"];
          var total_try = parseInt(howmany / 100) + 1;
          var datass = []

          // total_try 만큼 요청보내기
          for (var trying = 0; trying < total_try; trying++) {
            fetch(
              `${BASE_URL}/users/${id}/events?namespaces&per_page=100&page=${trying+1}&private_token=${token}`
            )
              .then(res => {
                return res.json();
              })

              .then(data => {
                data.forEach(function(value){
                  datass.push(value)
                })
                drawChart(datass)
              });
          } // for문 끝 요청부분 완료
        }
      } // callback 함수 완료
      request(options, callback);
    }, // getRepose 끝

    // ★★★★★★★★★  프로젝트 commit 가져오는 함수
    getCommits(id) {
      var request = require("request");
      var headers = { "PRIVATE-TOKEN": "5yRamVkqs4Z4bq-G1roY" };
      var options = {
        url: "https://lab.ssafy.com/api/v4/projects/6097/repository/commits?",
        headers: headers
      };

      function callback(error, response, body) {
        if (!error && response.statusCode == 200) {
          var howmany = response.headers["x-total"];
          var total_try = parseInt(howmany / 100) + 1;

          for (var trying = 0; trying < total_try; trying++) {
            fetch(
              `${BASE_URL}/projects/6097/repository/commits??namespaces&per_page=100&page=${trying+1}&private_token=5yRamVkqs4Z4bq-G1roY`
            )
              .then(res => {
                return res.json();
              })
              .then(data => {
                this.ret = data;
                console.log(data);
              });
          } // 요청부분 완료
        }
      } // callback 함수 정의 완료
      request(options, callback);
    } // getcommits 함수 완료
  } // methods 정의 완료
};
</script>

<style>


@font-face { font-family: 'BBTreeGR'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_nine_@1.1/BBTreeGR.woff') format('woff'); font-weight: normal; font-style: normal; }
.calendarContainer {
  position: relative;
  width: 100px;
  height: 250px;
}

.calendarBox {
  position: absolute;
  top: 10%;
  left: 1%;
  margin: auto;
}
</style>

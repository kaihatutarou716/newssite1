<template>
  <div class="wsList">
    <div class="wsBox">
      <div class="wsBox-Title">{{wsBoxTitle}}</div>
      <div
        class="weatherBox"
        v-for="(item, index) in whterList"
        v-bind:key="index"
      >
        <h1>{{whterList[index][0]}}</h1>
        <a
          class="currentCondition"
          :href="whterList[index][3]"
        >
          <div
            class="inline-Block"
            style="padding-right: 10px;"
          >
            <img
              class="imgIcon"
              :src="whterList[index][4]"
              data-icon="10"
            >
          </div>
          <div class="inline-Block">
            <span class="temp">{{whterList[index][1]}}</span>
          </div>
          <p class="description">{{whterList[index][2]}}</p>
        </a>
        <br>
      </div>
      <br>
      <div id="stockPriceBox">
        <table border="3">
          <tbody>
            <td
              colspan="2"
              align="center"
              class="stockPriceItem"
            >
              <h2>{{stockPriceItem}}</h2>
            </td>
            <tr
              class="stockPriceName"
              align="center"
            >
              <td>
                <h5>{{spList[0]}}</h5>
              </td>
              <td
                class="stockPrice"
                align="center"
              >
                <div>{{spList[1]}}<br>
                </div>
              </td>
            </tr>
            <tr
              class="stockPriceName"
              align="center"
            >
              <td>
                <h5>{{spList[2]}}</h5>
              </td>
              <td
                class="stockPrice"
                align="center"
              >
                <div>{{spList[3]}}<br>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
function arglist() {
  var list = [["東京", 130010], ["大阪", 270000], ["名古屋", 230010]];
  return list;
}

export default {
  data() {
    return {
      wsBoxTitle: "本日の天気・平均株価",
      C: "°C",
      whterList: [],
      stockPriceItem: "株価",
      spList: []
    };
  },

  headers: {
    "Content-Type": "application/json",
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Credentials": "true",
    "Access-Control-Allow-Methods": "POST, PUT, GET, DELETE, OPTIONS"
  },

  methods: {
    async getWhterData(list) {
      var arr = new Array();
      for (var i = 0; i < list.length; i++) {
        const baseUrl =
          "http://weather.livedoor.com/forecast/webservice/json/v1?city=";
        const url = baseUrl + list[i][1];
        const axios = require("axios");
        const res = await axios.get(url, { headers: this.headers });
        const items = res.data;
        var tempArr = [];
        tempArr[0] = list[i][0];
        tempArr[1] = items.forecasts[0].telop;
        tempArr[2] = items.description.text.slice(0, 60).replace(/\r?\n/g, "");
        tempArr[3] = items.link;
        tempArr[4] = items.forecasts[0].image.url;
        arr.push(tempArr);
      }
      this.whterList = arr;
    },

    async getStockPrice() {
      var arr = new Array();
      const axios = require("axios");
      const url =
        "https://api.matumo.com/kabu/kabu_ave_realtime_api_v1.php?type=0";
      const res = await axios.get(url);
      const resArray = res.data.split(",");
      arr.push("日経平均株価", resArray[4]);
      arr.push("NYダウ平均株価", resArray[7]);
      this.spList = arr;
    }
  },

  created() {
    var list = arglist();
    this.getWhterData(list);
    this.getStockPrice();
  }
};
</script>

<style scoped>
.wsList {
  background-color: #f6f7f9;
  border: 2px solid rgba(128, 128, 128, 1);
  letter-spacing: 0.3px;
}
.wsBox {
  padding-bottom: 10px;
}

.wsBox-Title {
  color: #fff;
  font-weight: bold;
  background-color: rgba(128, 128, 128, 1);
  padding: 4px 6px;
  text-align: center;
}

.inline-Block {
  display: inline-block;
  font-size: 30px;
  vertical-align: top;
}

.temp {
  padding-top: 50px;
}

.description {
  padding-left: 10px;
  display: inline-block;
  font-size: 20px;
  vertical-align: top;
}

.stockPrice .stockPrice {
  display: inline-block;
  vertical-align: top;
  text-align: top;
}

table {
  width: 80%;
  margin-left: auto;
  margin-right: auto;
  border-collapse: collapse;
  border: 1px solid rgba(128, 128, 128, 1);
}

.stockPriceItem {
  padding: 6px;
  background-color: rgba(128, 128, 128, 1);
  color: #ffffff;
  font-weight: normal;
  text-align: center;
  vertical-align: top;
}

table tr td {
  padding: 6px;
  background-color: #fff;
  border: 1px solid #666666;
}
</style>
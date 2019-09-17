<template>
  <div class="artleList">
    <div class="artleBox">
      <div class="artleBox-Title">{{artleBoxTitle}}
        <select
          class="pullDown"
          v-model="selected"
          v-on:change="updateProcess"
        >
          <option
            v-for="option in options"
            v-bind:value="option.value"
            v-bind:key="option.value"
          >
            {{ option.text }}
          </option>
        </select>
      </div>
      <div
        class="artleBox-Text"
        v-if="disFlg == true"
      >
        <dl>
          <div
            v-for="(item, index) in items "
            v-bind:key="index"
          >
            <dt>{{items[index][1]}}</dt>
            <dd>{{items[index][2]}}</dd>
          </div>
        </dl>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      artleBoxTitle: "選択データを表示",
      disFlg: false,
      items: [],
      selected: 0,
      options: [
        { text: "選択してください", value: 0 },
        { text: "地震情報", value: 1 }
      ]
    };
  },

  methods: {
    async getEqData(select) {
      var url = null;
      if (select == 0) {
        return;
      } else if (select == 1) {
        url = "https://api.p2pquake.net/v1/human-readable";
      }
      const axios = require("axios");
      const res = await axios.get(url);
      const items = res.data;
      if (items.length >= 1) {
        var arr = new Array();
        for (var i = 0; i < items.length; i++) {
          if (items[i].code == 551) {
            var eqArr = [];
            eqArr[0] = "地震";
            eqArr[1] = items[i].time;
            eqArr[2] =
              "震源地:" +
              items[i].earthquake.hypocenter.name +
              "\t" +
              "マグニチュード:" +
              items[i].earthquake.hypocenter.magnitude;
            arr.push(eqArr);
          }
        }
      } else {
        alert("地震情報はありません");
      }
      this.items = arr;
    },

    updateProcess() {
      const select = this.selected;
      if (select == 1) {
        this.getEqData(select);
        this.disFlg = true;
      } else if (select == 0) {
        this.disFlg = false;
      }
    }
  }
};
</script>

<style scoped>
.artleBox-Text {
  background-color: #f6f7f9;
  border: 2px solid rgba(128, 128, 128, 1);
  letter-spacing: 0.3px;
}

.artleBox-Title {
  color: #fff;
  font-weight: bold;
  background-color: rgba(128, 128, 128, 1);
  padding: 4px 6px;
  text-align: center;
}

.artleBox-Text dl dt {
  margin-top: 0;
}

.artleBox-Text dl dt {
  margin-top: 10px;
}

.artleBox-Text dl dd {
  margin-top: -1.3em;
  padding-left: 12.7em;
  line-height: 1.3;
}

.pullDown {
  float: right;
  margin-right: 6px;
}
</style>
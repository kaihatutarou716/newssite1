<template>
  <div id="qiitaList">
    <div class="qiitaBox">
      <div class="qiitaBox-Title">{{artleBoxTitle}}</div>
      <div
        class="qiitaBox-Text"
        v-if="disFlg == true"
      >
        <dl
          v-for='(item, index) in 5'
          :key='index'
        >
          <dt>{{items[index].updated_at.slice(0,-15)}}</dt>
          <dd><a
              :href="items[index].url"
              target="_blank"
            >{{items[index].title}}
            </a></dd>
        </dl>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      artleBoxTitle: "Qiita最新情報",
      disFlg: false,
      items: []
    };
  },

  methods: {
    getArtData() {
      const axios = require("axios");
      const url = "http://qiita.com/api/v2/items?page=1&amp;per_page=20";
      axios
        .get(url)
        .then(res => {
          this.items = res.data;
          this.disFlg = true;
        })
        .catch(function(err) {
          alert(err);
        });
    }
  },

  created() {
    var self = this;
    self.getArtData();
  }
};
</script>
  

<style scoped>
.qiitaBox {
  background-color: #f6f7f9;
  border: 2px solid rgba(128, 128, 128, 1);
  letter-spacing: 0.3px;
}

.qiitaBox-Title {
  color: #fff;
  font-weight: bold;
  background-color: rgba(128, 128, 128, 1);
  padding: 4px 6px;
  text-align: center;
}

.qiitaBox-Text dl dt {
  margin-top: 0;
}

.qiitaBox-Text dl dt {
  margin-top: 10px;
}

.qiitaBox-Text dl dd {
  margin-top: -1.3em;
  padding-left: 3.7em;
  line-height: 1.3;
}
</style>

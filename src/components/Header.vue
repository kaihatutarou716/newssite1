<template>
  <header>
    <h1>{{title}}</h1>
    <p>{{subTitle}}</p>
    <div class="content">
      <ul class="opstinTabs">
        <li><input
            type="radio"
            id="trigger"
            v-model="radioinfo"
            value="Qiita"
          >Qiita</li>
        <li><input
            type="radio"
            id="trigger"
            v-model="radioinfo"
            value="Books"
          >Books</li>
      </ul>
      <div>
        <label><input
            class="searchForm-input"
            type="text"
            v-model="seachText"
            @keyup.enter="dataSend"
          ></label>
        <label><button
            class="searchForm-submit"
            type="submit"
            @click="dataSend"
          ></button></label>
      </div>
      <hr>
    </div>
  </header>
</template>
 
<script>
export default {
  data() {
    return {
      title: "NY_informationSite",
      subTitle: "検索キーワードを入力してください",
      seachText: "",
      radioinfo: ""
    };
  },

  methods: {
    async dataSend() {
      if (
        this.radioinfo === null ||
        this.radioinfo === undefined ||
        this.seachText === ""
      ) {
        alert("ラジオボタンを選択してください");
        return;
      }
      if (
        this.seachText === null ||
        this.seachText === undefined ||
        this.seachText === ""
      ) {
        alert("検索キーワードを入力してください");
        return;
      }
      const axios = require("axios");
      const url =
        "https://www.googleapis.com/books/v1/volumes?q=intitle:プラチナエンド&Country=JP";
      const res = await axios.get(url);
      const items = res.data;
      console.log(JSON.stringify(items));
    }
  }
};
</script>
 
<style>
header {
  text-align: center;
  padding: 10px;
}

h1 {
  color: #013892;
  margin: 0;
}

p {
  color: #999;
  margin: 0;
}

ul.opstinTabs {
  font-size: 0;
  margin-right: 450px;
}

ul.opstinTabs li {
  display: inline-block;
  vertical-align: top;
  font-size: 12px;
  padding-right: 10px;
  padding-bottom: 10px;
}

.inpText {
  display: flex;
  justify-content: center;
  height: 70px;
  padding: 0px;
  box-sizing: border-box;
}

.searchForm-input {
  height: 40px;
  border: solid 1px #ddd;
  width: 600px;
  font-size: 14px;
  background-color: inherit;
  border-radius: 25px;
  font-family: "AxisStd-Regular", "Helvetica Neue", "Hiragino Kaku Gothic ProN",
    YuGothic, "Yu Gothic", Verdana, Meiryo, sans-serif;
}

.searchForm-submit {
  position: absolute;
  margin-left: 10px;
  width: 50px;
  height: 38px;
  border-radius: 0 4px 4px 0;
  background: #999;
}

.searchForm-submit::before {
  position: absolute;
  content: "";
  width: 15px;
  height: 15px;
  top: calc(50% - 9px);
  left: calc(50% - 9px);
  border-radius: 50%;
  box-shadow: 0 0 0 2px #fff;
}

.searchForm-submit::after {
  position: absolute;
  content: "";
  width: 8px;
  height: 6px;
  top: calc(50% + 6px);
  left: calc(50% + 2px);
  border-top: solid 2px #fff;
  transform: rotate(45deg);
}

.content {
  padding: 10px;
}
</style>
<template>
  <div id="app">
    <div class="vue_radio" v-if="show == 'menu'">
      <div class="tabs">
        <input type="radio" id="DrinkTab" value="1" v-model="isActive" />
        <label for="DrinkTab">ドリンク</label>
        <input type="radio" id="DessertTab" value="2" v-model="isActive" />
        <label for="DessertTab">デザ｜ト</label>
        <input type="radio" id="SetmealTab" value="3" v-model="isActive" />
        <label for="SetmealTab">定食</label>
         <input type="radio" id="SetmealTab" value="3" v-model="isActive" />
        <label for="SetmealTab" @click="TableShow">ホ｜ム</label>
      </div>
      <ul class="contents">
        <span class="tablenameno">
          <p>{{tablemember}}名様</p>
          <p>{{tableno}}卓</p>
        </span>
        <li v-if="isActive === '1'">
          <button class="LiItem" v-for="(list, index) in OrderDrink" :key="index" :class="list.temperature">
            <img :src="list.img" :alt="list.order_name" @click="SubTotal(list.full_name, list.price)"/>
            <p>{{list.order_name}}{{list.price}}</p>
          </button>
        </li>
        <li v-if="isActive === '2'" >
          <button class="LiItem" v-for="(list, index) in OrderDessert" :key="index" :class="list.temperature">
            <img :src="list.img" :alt="list.order_name" @click="SubTotal(list.full_name, list.price)"/>
            <p>{{list.order_name}}{{list.price}}</p>
          </button>
        </li>
        <li v-if="isActive === '3'">
          <button class="LiItem" v-for="(list, index) in OrderSetMeal" :key="index" :class="list.temperature">
            <img :src="list.img" :alt="list.order_name"  @click="SubTotal(list.full_name, list.price)"/>
            <p>{{list.order_name}}{{list.price}}</p>
          </button>
        </li>
        <button class="pay" @click="subtotal">
          小計
        </button>
      </ul>
    </div>
    <Subtotal v-else-if ="show ==='total'" @getChildtrue="OrderShow" :subtotalParent="subtotalList" :TableMember="tablemember" :TableNo="tableno" @getsubtotallist="listReset"></Subtotal>

    <TebleGesto v-else-if ="show ==='table'" @Show="Childshow" @Tablelist="ChildSubTotal" @TableNo_table="TableNo" @TableMember_member="TableMember"
    :TableMember="tablemember" :TableNo="tableno" 
    :subtotalParent="subtotalList"
    ></TebleGesto>
  </div>
</template>

<script>
import Subtotal from "./components/Subtotal";
import TebleGesto from "./components/TebleGesto";
  export default {
    components: {
     Subtotal,
     TebleGesto,
    },
    data() {
      return {
        isActive: "1",
        show: 'menu',
        // show: 'table',
        tablemember: "",
        tableno: "",
        subtotalList:[],
        OrderDrink: [
          {order_name: "HC", price:280, full_name: "ブレンドコーヒ", temperature: "hot", img: require("./assets/coffee01_blend.png")},
          {order_name: "HT", price:300, full_name: "紅茶", temperature: "hot", img: require("./assets/coffee03_cafeole.png")},
          {order_name: "ラテ", price:350, full_name: "カフェラテ", temperature: "hot", img: require("./assets/drink_tea_chai.png")},
          {order_name: "IC", price:300,full_name: "アイスコーヒー", temperature: "ice", img: require("./assets/coffee10_iced_coffee.png")},
          {order_name: "Iラテ", price:350,full_name: "アイスラテ", temperature: "ice", img: require("./assets/coffee11_iced_milk_coffee.png")},
          {order_name: "CF", price:550,full_name: "コーヒーフロート", temperature: "ice", img: require("./assets/drink_coffee_float.png")}
        ],

        OrderDessert: [
          {order_name: "アイス", price:300,full_name: "バニラアイス", temperature: "ice", img: require("./assets/sweets_icecream01_vanilla.png")},
          {order_name: "ケーキ", price:330,full_name: "いちごケーキ", temperature: "ice", img: require("./assets/sweets_shortcake.png")},
          {order_name: "パフェ", price:580,full_name: "いちごパフェ", temperature: "ice", img: require("./assets/sweets_pafe_parfait_ichigo.png")},
          {order_name: "set Hot", price:120,full_name: "セットコーヒー", temperature: "hot", img: require("./assets/coffee01_blend.png")},
          {order_name: "set Ice", price:300,full_name: "セットアイス", temperature: "ice", img: require("./assets/coffee10_iced_coffee.png")}
        ],
        
        OrderSetMeal: [
          {order_name: "カレー", price:680,full_name: "ビーフカレー", temperature: "hot", img: require("./assets/food_curryrice_white.png")},
          {order_name: "カツ", price:780, full_name: "ソースカツ定食", temperature: "hot",img: require("./assets/food_kanazawa_curry.png")},
          {order_name: "天津丼", price:700,  full_name: "天津丼", temperature: "hot",img: require("./assets/food_kanitama_tenshinhan.png")},
          {order_name: "かつ丼", price:880,full_name: "ソースかつ丼", temperature: "hot", img: require("./assets/food_sauce_katsudon_fukui.png")},
          {order_name: "うなぎ", price:1000,full_name: "うな重", temperature: "hot", img: require("./assets/food_unagi_ochaduke.png")},
          {order_name: "スパ", price:880,full_name: "ボンゴレ", temperature: "hot", img: require("./assets/food_spaghetti_vongole_bianco.png")},
          {order_name: "担々麵", price:650,full_name: "担々麺", temperature: "hot", img: require("./assets/ramen_top_tantanmen.png")},
          {order_name: "ラーメン", price:320,full_name: "豚骨ラーメン", temperature: "hot", img: require("./assets/ramen_top_tonkotsu.png")},
          {order_name: "お好み", price:680,full_name: "お好み焼き", temperature: "hot", img: require("./assets/food_okonomiyaki_kyabetsuyaki_decoration.png")},
          {order_name: "餃子", price:250,full_name: "餃子", temperature: "hot", img: require("./assets/food_gyouza_age.png")}
        ],
      };
    },
    methods: {
      subtotal() {
        this.show = "total";
      },
      OrderShow() {
        this.show = "menu";
      },
      TableShow() {
        this.show = "table";
      },
      
      Childshow(show) {
        this.show = show
      },

      SubTotal(full_name, child_price) {
        this.subtotalList.push({name:full_name, price:child_price})
      },

      ChildSubTotal(tablelist) {
        for(let i=0; i <= tablelist.length; i++){
          let table_name = tablelist[i].name;
          let table_price = tablelist[i].price;
          this.SubTotal(table_name, table_price)
        }
      },

      TableNo(no) {
        this.tableno = no;
      },

      TableMember(no) {
        this.tablemember = no;
      },
      //子から受け取ったlistResetの中身はfalse 着火のみで使用
      listReset() {
        return this.subtotalList = [];
      }
    }
  };
</script>
<style>
html, body, div, span, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
abbr, address, cite, code,
del, dfn, em, img, ins, kbd, q, samp,
small, strong, sub, sup, var,
b, i,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, figcaption, figure,
footer, header, hgroup, menu, nav, section, summary,
time, mark, audio, video {
  margin:0;
  padding:0;
  border:0;
  outline:0;
  font-size:100%;
  vertical-align:baseline;
  background:transparent;
}

body {
  line-height:1;
}

article,aside,details,figcaption,figure,
footer,header,hgroup,menu,nav,section {
  display:block;
}

nav ul {
  list-style:none;
}

blockquote, q {
  quotes:none;
}

blockquote:before, blockquote:after,
q:before, q:after {
  content:'';
  content:none;
}

a {
  margin:0;
  padding:0;
  font-size:100%;
  vertical-align:baseline;
  background:transparent;
}

/* change colours to suit your needs */
ins {
  background-color:#ff9;
  color:#000;
  text-decoration:none;
}

/* change colours to suit your needs */
mark {
  background-color:#ff9;
  color:#000;
  font-style:italic;
  font-weight:bold;
}

del {
  text-decoration: line-through;
}

abbr[title], dfn[title] {
  border-bottom:1px dotted;
  cursor:help;
}

table {
  border-collapse:collapse;
  border-spacing:0;
}

/* change border colour to suit your needs */
hr {
  display:block;
  height:1px;
  border:0;
  border-top:1px solid #cccccc;
  margin:1em 0;
  padding:0;
}

input, select {
  vertical-align:middle;
}

/*-----------------------*/
/*         共通設定       */
/*-----------------------*/
button{
  background-color: transparent;
  border: none;
  cursor: pointer;
  outline: none;
  padding: 0;
  appearance: none;
}



/*-----------------------*/
/*          tab          */
/*-----------------------*/
#app {
  width: auto;
  height: 736vh;
  background-color: #F5F5F4;
  z-index: -999;
}

.contents {
  background-color:#fff;
}

.tabs {
  background-color: #666;
  color: #fff;
  position: absolute;
  left: 83%;
  top: 2%;
}

.tabs label {
  padding: 5px 15px;
  cursor: pointer;
  display: block;
  width: 20px;
  height: 123.12px;
  font-family: Roboto;
  font-size: 20px;
  line-height: 1.5em;
}

.tabs label:nth-of-type(2) {
  position: absolute;
  top:150px;
  background-color: #666;
  color: #fff;
}

.tabs label:nth-of-type(3) {
  position: absolute;
  top:300px;
  background-color: #666;
  color: #fff;
}

.tabs label:nth-of-type(4) {
  position: absolute;
  top:450px;
  background-color: #666;
  color: #fff;
}

.tabs :checked + label {
  background-color: rgb(255, 255, 255);
  color: rgb(0, 0, 0);
}

input {
  display: none;
}

.tablenameno {
  display: flex;
  padding: 5px 0 0 30px;
}

.tablenameno p:nth-of-type(2) {
  padding-left: 20px;
}

/*-----------------------*/
/*      メニュー類        */
/*-----------------------*/
.contents {
  position: relative;
  width: 80vw;
  height: 95vh;
  top: 15px;
  left: 15px;
}

ul {
  margin: 0;
  padding: 0;
}

.contents li {
  list-style: none;
  width: auto;
  padding: 10px 10px 30px 5px;
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
  padding-bottom: 10px;
}

/*最後の行が1個の場合、左詰めの見えないbox*/
.contents li:before {
  content:"";
  display: block;
  width:83.72px;
  order:1;
}

 /*最後の行が2個の場合、左詰めの見えないbox*/
.contents li:after {
  display: block;
  content:"";
  width: 83.72px;
}

.contents .LiItem {
  width: 83.72px;
  height: 98.5px;
  border: 1px #000 solid;
  text-align: center;
  margin-bottom: 20px;
}

.contents .hot {
  background-color: antiquewhite;
}

.contents .ice {
  background-color: aliceblue;
}

.contents img {
  object-fit: cover;
  width: 75.11px;
  height: 72.64px;
}

.LiItem p {
  white-space: nowrap;
  font-size: 100%;
}

.pay {
  width: 206px;
  height: 54px;
  background: #7DCBD0;
  border-radius: 74px;

  font-family: Roboto;
  font-weight: bold;
  font-size: 30px;
  line-height: 42px;

  color: #FFFFFF;

  position: absolute;
  left: 50%;
  top: 85vh;
  transform: translate(-50%, -50%);
  -webkit-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
}

</style>

<template>
  <div id="TebleGesto">
    <div class="vue_radio">
      <div class="tabs">
        <input type="radio" v-model="isActive" />
        <label for="DrinkTab" @click="SubTotal_show">小計</label>
        <input type="radio" v-model="isActive" />
        <label for="DessertTab" @click="OrderSystem_show">注文</label>
        <input type="radio" v-model="isActive"/>
        <label for="SetmealTab" @click="SubTotal_show">取り消し</label>
      </div>

      <ul class="contents">
        <p class="time">{{year}}/{{mouth}}/{{day}} {{hours}}:{{minutes}}:{{second}}</p>
        <span class="tablenameno">
          <p>{{tablemember}}名様</p>
          <p>{{tableno}}卓</p>
        </span>
        <form class="table_form">
          人数:<input type="tel" class="table_no_look" v-model="tablemember" maxlength="2" oninput="value = value.replace(/[^0-9]+/i,'');" />
        </form>

        <form class="table_form">
          席番:<input type="tel" class="table_no_look" v-model="tableno"  maxlength="2" oninput="value = value.replace(/[^0-9]+/i,'');" />
        </form>


        <div class="tabs_table">
          <input type="radio" id="DrinkTab" value="1" v-model="isActive" />
          <label for="DrinkTab"><p>ドリンク</p></label>
          <input type="radio" id="DessertTab" value="2" v-model="isActive" />
          <label for="DessertTab"><p>デザート</p></label>
          <input type="radio" id="SetmealTab" value="3" v-model="isActive" />
          <label for="SetmealTab"><p>定食</p></label>
        </div>
      <perfect-scrollbar class="psTabel">

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
      </perfect-scrollbar>
      </ul>

    </div>
  </div>
</template>

<script>
import { PerfectScrollbar } from "vue2-perfect-scrollbar";
  export default {
    data() {
      return {
        isActive: "2",
        show: "",
        tableno: "",
        tablemember: "",
        subtotalList:[],
        year: new Date().getFullYear(),
        mouth: new Date().getMonth() + 1,
        day: new Date().getDate(),
        hours: new Date().getHours(),
        minutes: new Date().getMinutes(),
        second: new Date().getSeconds(),
        OrderDrink: [
          {order_name: "HC", price:280, full_name: "ブレンドコーヒ", temperature: "hot", img: require("../assets/coffee01_blend.png")},
          {order_name: "HT", price:300, full_name: "紅茶", temperature: "hot", img: require("../assets/coffee03_cafeole.png")},
          {order_name: "ラテ", price:350, full_name: "カフェラテ", temperature: "hot", img: require("../assets/drink_tea_chai.png")},
          {order_name: "IC", price:300,full_name: "アイスコーヒー", temperature: "ice", img: require("../assets/coffee10_iced_coffee.png")},
          {order_name: "Iラテ", price:350,full_name: "アイスラテ", temperature: "ice", img: require("../assets/coffee11_iced_milk_coffee.png")},
          {order_name: "CF", price:550,full_name: "コーヒーフロート", temperature: "ice", img: require("../assets/drink_coffee_float.png")}
        ],

        OrderDessert: [
          {order_name: "アイス", price:300,full_name: "バニラアイス", temperature: "ice", img: require("../assets/sweets_icecream01_vanilla.png")},
          {order_name: "ケーキ", price:330,full_name: "いちごケーキ", temperature: "ice", img: require("../assets/sweets_shortcake.png")},
          {order_name: "パフェ", price:580,full_name: "いちごパフェ", temperature: "ice", img: require("../assets/sweets_pafe_parfait_ichigo.png")},
          {order_name: "set Hot", price:120,full_name: "セットコーヒー", temperature: "hot", img: require("../assets/coffee01_blend.png")},
          {order_name: "set Ice", price:300,full_name: "セットアイス", temperature: "ice", img: require("../assets/coffee10_iced_coffee.png")}
        ],
        
        OrderSetMeal: [
          {order_name: "カレー", price:680,full_name: "ビーフカレー", temperature: "hot", img: require("../assets/food_curryrice_white.png")},
          {order_name: "カツ", price:780, full_name: "ソースカツ定食", temperature: "hot",img: require("../assets/food_kanazawa_curry.png")},
          {order_name: "天津丼", price:700,  full_name: "天津丼", temperature: "hot",img: require("../assets/food_kanitama_tenshinhan.png")},
          {order_name: "かつ丼", price:880,full_name: "ソースかつ丼", temperature: "hot", img: require("../assets/food_sauce_katsudon_fukui.png")},
          {order_name: "うなぎ", price:1000,full_name: "うな重", temperature: "hot", img: require("../assets/food_unagi_ochaduke.png")},
          {order_name: "スパ", price:880,full_name: "ボンゴレ", temperature: "hot", img: require("../assets/food_spaghetti_vongole_bianco.png")},
          {order_name: "担々麵", price:650,full_name: "担々麺", temperature: "hot", img: require("../assets/ramen_top_tantanmen.png")},
          {order_name: "ラーメン", price:320,full_name: "豚骨ラーメン", temperature: "hot", img: require("../assets/ramen_top_tonkotsu.png")},
          {order_name: "お好み", price:680,full_name: "お好み焼き", temperature: "hot", img: require("../assets/food_okonomiyaki_kyabetsuyaki_decoration.png")},
          {order_name: "餃子", price:250,full_name: "餃子", temperature: "hot", img: require("../assets/food_gyouza_age.png")}
        ],
      };
    },
    props:["subtotalParent", "TableMember", "TableNo"],

    methods: {
      subtotal() {
        this.show = false;
      },
      OrderShow() {
        this.show = true;
      },
      SubTotal(full_name, child_price) {
        this.subtotalList.push({name:full_name, price:child_price})
      },

      SubTotal_show() {
        this.$emit("Show", "total");
        this.$emit("Tablelist", this.subtotalList);
        this.$emit("TableNo", this.tableno);
        this.$emit("TableMember", this.tablemember)
      },

      OrderSystem_show(){
        this.$emit("Show", "menu");
        this.$emit("Tablelist", this.subtotalList);
        this.$emit("TableNo_table", this.tableno);
        this.$emit("TableMember_member", this.tablemember)
      },

      //子から受け取ったlistResetの中身はfalse 着火のみで使用
      listReset() {
        return this.subtotalList = [];
      }
    },
    // computed: {
    //   ChildSubTotal() {
    //     let list = this.subtotalParent
    //     for(let i=0; i <= list.length; i++){
    //       let table_name = list[i].name;
    //       let table_price = list[i].price;
    //       this.SubTotal(table_name, table_price)
    //     }
    //   }
    // },
    components: {
    PerfectScrollbar
  }
  };
</script>
<style src="vue2-perfect-scrollbar/dist/vue2-perfect-scrollbar.css"/>

<style>
/*-----------------------*/
/*          tab          */
/*-----------------------*/
#TebleGesto {
  width: auto;
  height: 100vh;
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

.tabs_table {
  background-color: #666;
  color: #fff;
  position: absolute;
  z-index: 1;
  top:44.5vh;
  left: 20vw;
  
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

.tabs_table label {
width: 80px;
height: 23px;

font-family: Roboto;
font-size: 20px;
color: #ffffff;
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

.tabs_table label:nth-of-type(1) {
  position: absolute;
  color: #fff;
  width: 80px;
  border-bottom: 30px solid #666;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  height: 0;
  text-align: center;
  line-height: 30px;

  top: -25px;
  left: -70px;

}

.tabs_table label:nth-of-type(2) {
  position: absolute;
  color: #fff;
  width: 80px;
  border-bottom: 30px solid #666;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  height: 0;
  text-align: center;
  line-height: 30px;

  top: -25px;;
  left: 130px;

}

.tabs_table label:nth-of-type(3) {
  position: absolute;
  color: #fff;
  width: 80px;
  border-bottom: 30px solid #666;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  height: 0;
  text-align: center;
  line-height: 30px;

  top: -25px;;
  left: 30px;
}

.tabs :checked + label {
  background-color: rgb(255, 255, 255);
  color: rgb(0, 0, 0);
}

.tabs_table :checked + label {
  border-bottom: 30px solid rgb(255, 255, 255);
  border-left: 10px solid  transparent;
  border-right: 10px solid  transparent;
  color: rgb(0, 0, 0);
}

input {
  display: none;
}

.time {
    font-size: 14px;
    padding: 5px;
}

.table_form {
  text-align: right;
  margin-top: 5vh;
  margin-right: 8vw;
  font-size: 20px;  
}

.table_no_look {
  display: inline-block;
  border: none;
  text-align: right;
  border-bottom: 1px #000 solid;
  width: 15vw;
  margin-left: 3vw;
  font-size: 20px;
}

.tablenameno {
  display: flex;
  padding-left: 10px;
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

.psTabel {
  position: absolute;
  top: 45vh;
  height: 45vh;
  border-top: 2px solid #666;
}

</style>

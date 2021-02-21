<template>
  <div id="TebleGesto">
    <div class="vue_radio">
      <div class="tabs">
        <input type="radio" v-model="isActive" />
        <label for="DrinkTab" @click="SubTotal_show">小計</label>
        <input type="radio" v-model="isActive" />
        <label for="DessertTab" @click="OrderSystem_show">注文</label>
        <input type="radio" v-model="isActive"/>
        <label for="SetmealTab" @click="SubTotal_show('visible')">取り消し</label>
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


        <span class="tabs_table">
          <input type="radio" id="DrinkTab" value="1" v-model="isActive" />
          <label for="DrinkTab"><p>ドリンク</p></label>
          <input type="radio" id="DessertTab" value="2" v-model="isActive" />
          <label for="DessertTab"><p>デザート</p></label>
          <input type="radio" id="SetmealTab" value="3" v-model="isActive" />
          <label for="SetmealTab"><p>定食</p></label>
        </span>
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
        isActive: "2", //ラジオボタンの判定
        show: "", //ページの判定[menu][table][subtotal]
        tableno: this.TableNo, //人数
        tablemember: this.TableMember, //テーブル番号
        subtotalList:[], //商品ボタンを押した際、追加
        
        //年月日時間の表記
        year: new Date().getFullYear(),
        mouth: new Date().getMonth() + 1,
        day: new Date().getDate(),
        hours: new Date().getHours(),
        minutes: new Date().getMinutes(),
        second: new Date().getSeconds(),
      };
    },
    //親からの受け取り
    props:["subtotalParent", //注文を受けた商品を入れたlist
           "TableMember", //人数
           "TableNo", //テーブル番号
           "OrderDrink", //各注文のリスト
           "OrderDessert", 
           "OrderSetMeal"
           ],

    methods: {
      // ホームからの商品ボタンの商品を追加
      SubTotal(full_name, child_price) {
        this.subtotalList.push({name:full_name, price:child_price})
      },
      // 小計の表示
      // 移動の際、各データを親に渡す
      SubTotal_show(test) {
        this.$emit("Show", "total");
        this.$emit("Tablelist", this.subtotalList);
        this.$emit("TableNo_table", this.tableno);
        this.$emit("TableMember_member", this.tablemember);
        if(test == "visible") {
          this.$emit("Delete", test)
        }
      },
      // 注文に移動
      // 移動の際、各データを親に渡す
      OrderSystem_show(){
        this.$emit("Show", "menu");
        this.$emit("Tablelist", this.subtotalList);
        this.$emit("TableNo_table", this.tableno);
        this.$emit("TableMember_member", this.tablemember);
      }
    },

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

.tabs_table {
  background-color: #666;
  color: #fff;
  position: absolute;
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
  justify-content: space-between;
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

/*-----------------------*/
/*       ~390px~          */
/*-----------------------*/
@media screen and (max-width: 390px) {
   .tabs_table {
    background-color: #666;
    color: #fff;
    position: absolute;
    top:44.5vh;
    left: 30vw;
  }

  .tabs_table label:nth-of-type(1) {
    position: absolute;
    color: #fff;
    width: 60px;
    border-bottom: 30px solid #666;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    height: 0;
    text-align: center;
    line-height: 30px;

    top: -25px;
    left: -80px;

  }

  .tabs_table label:nth-of-type(2) {
    position: absolute;
    color: #fff;
    width: 60px;
    border-bottom: 30px solid #666;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    height: 0;
    text-align: center;
    line-height: 30px;

    top: -25px;;
    left: 85px;

  }

  .tabs_table label:nth-of-type(3) {
    position: absolute;
    color: #fff;
    width: 60px;
    border-bottom: 30px solid #666;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    height: 0;
    text-align: center;
    line-height: 30px;

    top: -25px;
    left: 5px;
  }

  .tabs_table :checked + label {
    border-bottom: 30px solid rgb(255, 255, 255);
    border-left: 10px solid  transparent;
    border-right: 10px solid  transparent;
    color: rgb(0, 0, 0);
  }

  .tabs_table p{
    font-size: 4vw
  }
}


/*-----------------------*/
/*       481px~          */
/*-----------------------*/
@media screen and (min-width: 481px) {
  .tabs_table {
    background-color: #666;
    color: #fff;
    position: absolute;
    top:44.5vh;
    left: 30vw;
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
    left: -130px;

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

    top: -25px;
    left: 5px;
  }

  .tabs_table :checked + label {
    border-bottom: 30px solid rgb(255, 255, 255);
    border-left: 10px solid  transparent;
    border-right: 10px solid  transparent;
    color: rgb(0, 0, 0);
  }
}

/*-----------------------*/
/*       769px~          */
/*-----------------------*/
@media screen and (min-width: 769px) {
  .tabs_table {
    background-color: #666;
    color: #fff;
    position: absolute;
    top:44.5vh;
    left: 20vw;
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
    left: -130px;

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
    left: 90px;

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

    top: -25px;
    left: -20px;
  }

  .tabs_table :checked + label {
    border-bottom: 30px solid rgb(255, 255, 255);
    border-left: 10px solid  transparent;
    border-right: 10px solid  transparent;
    color: rgb(0, 0, 0);
  }

  .psTabel {
    position: absolute;
    top: 45vh;
    height: 45vh;
    width: 80vw;
    border-top: 2px solid #666;
  }
}

</style>

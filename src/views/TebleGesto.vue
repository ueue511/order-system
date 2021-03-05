<template>
    <div class="vue_radio">
      <Tabs 
        :Tabsnumchild = "tabs"
      />
      <ul class="contents">
        <TimeMenber />
        <form class="table_form">
          人数:<input type="tel" class="table_no_look" v-model="tablemember" maxlength="2" oninput="value = value.replace(/[^0-9]+/i,'');" />
        </form>
        <form class="table_form">
          席番:<input type="tel" class="table_no_look" v-model="tableno"  maxlength="2" oninput="value = value.replace(/[^0-9]+/i,'');" />
        </form>
        <span class="tabs_table">
          <input type="radio" id="DrinkTab" :value="1" v-model="isActive" />
          <label for="DrinkTab"><p>ドリンク</p></label>
          <input type="radio" id="DessertTab" :value="2" v-model="isActive" />
          <label for="DessertTab"><p>デザート</p></label>
          <input type="radio" id="SetmealTab" :value="3" v-model="isActive" />
          <label for="SetmealTab"><p>定食</p></label>
        </span>
      <perfect-scrollbar class="psTabel">
        <li>
          <Munebutton
            :MuneListNum="isActive"
          />
        </li>
      </perfect-scrollbar>
      </ul>
    </div>
</template>

<script>
import { PerfectScrollbar } from "vue2-perfect-scrollbar";
import Munebutton from "../components/Munebutton";
import Tabs from "../components/Tabs"
import TimeMenber from "../components/TimeMenber"
  export default {
    data() {
      return {
        isActive: 1, //ラジオボタンの判定
        tabs: "tablegesto",
        subtotalList:[], //商品ボタンを押した際、追加
        tableno: this.$store.state.tableno, //人数
        tablemember: this.$store.state.tablemember, //テーブル番号
      };
    },
    watch: {
      tableno: function(tableno){
        this.$store.commit("Table", tableno);
      },
      tablemember: function(tablemember){
        this.$store.commit("GestoMenber", tablemember);
      }
    },
    components: {
      PerfectScrollbar,
      Munebutton,
      Tabs,
      TimeMenber
    }
  };
</script>
<style src="vue2-perfect-scrollbar/dist/vue2-perfect-scrollbar.css"/>

<style>
/*-----------------------*/
/*          tab          */
/*-----------------------*/
.tabs_table {
  background-color: #666;
  color: #fff;
  position: absolute;
  top:44.5vh;
  left: 20vw;
}

.tabs_table label {
width: 80px;
height: 23px;

font-family: Roboto;
font-size: 20px;
color: #ffffff;
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

.tabs_table :checked + label {
  border-bottom: 30px solid rgb(255, 255, 255);
  border-left: 10px solid  transparent;
  border-right: 10px solid  transparent;
  color: rgb(0, 0, 0);
}

input {
  display: none;
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

/*-----------------------*/
/*      メニュー類        */
/*-----------------------*/
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

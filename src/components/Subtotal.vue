<template>
  <div id="SubTotal">
    <div class="vue_radio">
      <div class="tabs">
        <input type="radio" id="subtotal" value="1" v-model="isActive" />
        <label for="subtotal">小計</label>
        <input type="button" />

        <label for="DessertTab" @click="OrderSystem" >注文</label>

        <label for="SetmealTab" @click="Delete">取り消し</label>
      </div>

      <div class="contents_subtotal">
        <div class="subtotal" v-if="isActive === '1'">
          <span class="tablenameno">
            <p>{{year}}/{{mouth}}/{{day}} {{hours}}:{{minutes}}:{{second}}</p>
            <p>{{tablemember}}名様</p>
            <p>{{tableno}}卓</p>
          </span>
          
          <perfect-scrollbar>
            <button class="lain_click"  v-for="(list,index) in subtotalParent" :key="index">
              <div>{{list.name}}</div>
              <button class="deletebutton" @click.stop="DeleteMenu(index)" :style="{visibility: deleteTrue}">削除</button>
              <div>￥{{list.price}}</div>
            </button>
            <p class="lineBottom"></p>
            <ul id="product">
              <li>小計</li>
              <li>￥{{ total_all }}</li>
            </ul>
          </perfect-scrollbar>
        </div>
        <button class="pay" @click="openCloseModal(); send_type()">送信</button>
        <div id="overlay" v-if="show_modal">
          <div id="content_send">
            <button @click="openCloseModal"><span>{{ send }}</span></button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { PerfectScrollbar } from "vue2-perfect-scrollbar";
export default {
  data() {
    return {
      isActive: "1", //ラジオボタンの判定
      show: "", //ページの判定[menu][table][subtotal]
      tableno: this.TableNo, //人数
      tablemember: this.TableMember, //テーブル番号
      
      //年月日時間の表記
      year: new Date().getFullYear(),
      mouth: new Date().getMonth() + 1,
      day: new Date().getDate(),
      hours: new Date().getHours(),
      minutes: new Date().getMinutes(),
      second: new Date().getSeconds(),

      deleteTrue: this.DeleteSub, //取り消しボタンの表示 非表示
      show_modal: false, //送信ボタンを押した際、モーダルウィンドの表示判定
      delete_contents: [], //削除商品を格納するlist
    };
  },
  //親からの受け取り
  props: ["subtotalParent", //注文を受けた商品を入れたlist
          "TableMember", //人数
          "TableNo", //テーブル番号
          "DeleteSub" //ホームから取り消しボタンを押したか判定
          ],
  //注文リストの合計金額を計算する関数
  computed: {
    total_all() {
      let list = this.subtotalParent;
      let price_total = list.reduce(function(sum, element) {
        return sum + element.price;
        }, 0);
      return price_total;
    },
    // Delete_sub() {
    //   if(this.DeleteSub === "visible") {
    //     return this.DeleteSub
    //   }; 
    // }
  },

  methods: {
    //注文に移動
    OrderSystem() {
      this.$emit("getChildtrue");
    },
    //取り消しボタンを押し、削除ボタンの表示判定
    Delete() {
      if(this.deleteTrue === "hidden") {
        return this.deleteTrue = "visible"
      } else {
        return this.deleteTrue = "hidden";
      }
    },
    //削除ボタンを押した際、削除する関数
    DeleteMenu(index) {
      return this.subtotalParent.splice(index, 1)
    },
    //モーダルウィンド表示判定
    openCloseModal() {
      if(this.show_modal === true) {
        this.show_modal = false;
        this.$emit("getsubtotallist",false);
      } else {
        setTimeout(this.closeModal, 5000);
        this.show_modal = true
      }
    },
    //モーダルウィンドの非表示判定
    closeModal() {
      this.show_modal = false;
      this.$emit("getsubtotallist",false);
    },
    //モーダルウィンドの表示文章判定
    send_type() {
      if(this.subtotalParent.length === 0){
        this.send = "未入力"
      } else {
          this.send = "送信完了"
      }
    }
  },
  components: {
    PerfectScrollbar
  }
};
</script>
<style src="vue2-perfect-scrollbar/dist/vue2-perfect-scrollbar.css"/>

<style scoped>
/*-----------------------*/
/*          tab          */
/*-----------------------*/
#SubTotal {
  width: auto;
  height: 736px;
  background-color: #F5F5F4;
  z-index: -999;
}

.contents_subtotal {
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

.tabs :checked + label {
  background-color: rgb(255, 255, 255);
  color: rgb(0, 0, 0);
}

input {
  display: none;
}

.tablenameno {
  display: flex;
  padding-left: 5px;
  justify-content: space-between;
}

/*-----------------------*/
/*      メニュー類        */
/*-----------------------*/
.contents_subtotal {
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

/*-----------------------*/
/*          小計         */
/*-----------------------*/
.subtotal {
  width: 90%;
  height: 78vh;
  position: absolute;
  left: 16.5px;
  top: 10px;
}

.subtotal p {
  font-size: 14px;
  padding: 5px;
}

#product {
  display: flex;
  justify-content: space-between;
  padding: 30px 30px 0 30px;
  list-style: none;
}



.lain_click {
  display: flex;
  justify-content: space-between;
  padding: 10px 30px 10px 30px;
  width: 100%;
  font-size: 16px;
}

.lain_click >:first-child {
  margin-right: auto;
}

.lineThrough {
  text-decoration-line: line-through;
}

.lineBottom {
  margin: 80px 20px 0 20px;
  width: 85%;
  border-bottom: 1px #000 solid;
}

.deletebutton {
  background-color: #7DCBD0;
  border-radius: 74px;
  border: 1.5px solid #388388;
  width: 60px;
  color: rgb(255, 255, 255);
  cursor: pointer;
  padding: 0;
  appearance: none;
}

.ps {
  height: 70vh;
  border: 2px solid #666;
}

/*-----------------------*/
/*         ボタン         */
/*-----------------------*/
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

/*-----------------------*/
/*        モーダル        */
/*-----------------------*/
#overlay {
  z-index: 1;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}
#content_send {
  text-align: center;
  z-index: 2;
  width: 40%;
  padding: 20px;
  background: #ffffff;
  border-radius: 10px;
}

#content_send button {
  color: rgb(26, 26, 26);
  font-size: 20px;
  font-weight: bold;
}


</style>
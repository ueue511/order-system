<template>
  <div class="vue_radio">
    <Tabs 
      @TabNum="ShowNum"
      :Tabsnumchild = "tabs"
    />
    <ul class="contents">
      <TimeMenber />
      <Munebutton 
        :MuneListNum="isActive"
      />
      <router-link :to="{name: 'Subtotal'}">
        <button class="pay" @click="SubTotal_show('hidden')">
          小計
        </button>
      </router-link>
    </ul>
  </div>
</template>

<script>
import Munebutton from "../components/Munebutton";
import Tabs from "../components/Tabs"
import TimeMenber from '../components/TimeMenber.vue';
  export default {
    components: {
     Munebutton,
     Tabs,
     TimeMenber
    },
    data() {
      return {
        isActive: 1,
        tabs: 'home',
        subtotalList:[], //商品ボタンを押した際、追加
        subtotal_delete: "hidden", // 削除ボタンの判定
        tablemember: "",
        tableno: ""
      };     
    },
    
    methods: {
      // 移動の際 消去ボタン非表示
      SubTotal_show(show) {
        this.$store.commit('Delete', show)
      },
      ShowNum(isActiveChild) {
        return this.isActive=isActiveChild
      }
    },
  };
</script>
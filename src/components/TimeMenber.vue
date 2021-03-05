<template>
  <span class="tablenameno">
    <p>{{year}}/{{mouth}}/{{day}} {{hours}}:{{minutes}}:{{second}}</p>
    <p>{{tablemember}}名様{{tableno}}卓</p>
  </span>
</template>

<script>
export default {
  data() {
    return {
      tableno: this.$store.state.tableno, //人数
      tablemember: this.$store.state.tablemember, //テーブル番号

      //年月日時間の表記
      year: new Date().getFullYear(),
      mouth: new Date().getMonth() + 1,
      day: new Date().getDate(),
      hours: new Date().getHours(),
      minutes: new Date().getMinutes(),
      second: new Date().getSeconds(),
    }
  }
}
</script>

<style scoped>
.tablenameno {
  display: flex;
  padding-left: 5px;
  justify-content: space-between;
}
</style>
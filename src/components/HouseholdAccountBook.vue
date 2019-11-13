<template>
<div id="hab">

  <b-form @submit="onSubmit" @reset="onReset" v-if="show">
    <b-alert show>簡易家計簿</b-alert>

    <!-- select -->
    <b-form-group
      id="input-group-1"
      label="項目："
      label-for="select-1"
      description="購入した商品の項目を設定"
    >
      <b-form-select id="select-1" v-model="item" :options="options" required></b-form-select>
    </b-form-group>

    <!-- input number -->
    <b-form-group id="input-group-2" label="金額：" label-for="input-1">
      <b-form-input id="input-1" v-model="money" type="number" placeholder="金額を入力" required></b-form-input>
    </b-form-group>

    <!-- submit & reset -->
    <b-button type="submit" variant="primary">設定</b-button>
    <b-button type="reset" variant="danger">入力リセット</b-button>

    <!-- disp -->
    <b-card class="mt-3" header="入力項目の確認">
      <pre class="m-0">項目：{{ item }}</pre>
      <pre class="m-0">金額：{{ money }}円</pre>
    </b-card>

    <b-card class="mt-3" header="設定項目">
      <div v-html="submitList" class="m-0"></div>
      <div class="m-0">合計：{{ total }}円</div>
    </b-card>

  </b-form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HouseholdAccountBook',
  data: {
      // 初期化
      show: true,
      money: '',
      item: null,
        options: [
          { value: null, text: '商品項目を選択' },
          '日用品','雑貨','食費','娯楽費',
          '教育費','ペット費','習い事費','その他'
        ],
      submitList: '',
      total: '',
      totalNum: 0
  },
  methods: {
　　　// 設定ボタンの処理
      onSubmit(evt) {
        evt.preventDefault()
        //alert(JSON.stringify([this.item,this.money]))
        // 入力された項目を表示（追記）
        this.submitList += '<pre class="m-0">項目：' + this.item + '</pre><pre class="m-0">金額：' + this.money + '円</pre>'
　　　　// 合計金額の計算
        this.total = Number(this.totalNum) + Number(this.money)
        // 合計金額の保持
        this.totalNum = this.total
      },
      // リセットボタンの処理
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.item = null
        this.money = ''
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
button {
  margin: 10px 0;
  padding: 10px;
}
</style>

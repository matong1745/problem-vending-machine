<template>
  <div class="container">
    <h1>
      Beverage Vending Machine
    </h1>
    <div class="coinInsertContainer">
      <div
        class="coinOne"
        @click="add(1)"
      >
        1
      </div>
      <div
        class="coinTwo"
        @click="add(2)"
      >
        2
      </div>
      <div
        class="coinFive"
        @click="add(5)"
      >
        5
      </div>
      <div
        class="coinTen"
        @click="add(10)"
      >
        10
      </div>
    </div>
    <div class="wallet">
      <h2>Your Wallet</h2>
      <h3>{{ wallet }}</h3>
    </div>
    <div class="beveragesContainer">
      <BeverageCard
        v-for="item in beverages"
        :key="item.name"
        :item="item"
        :available="item.price <= wallet"
        @click="selectItem(item)"
      />
    </div>
    <div
      class="item"
      v-if="beverage"
    >
      <h2>Your Beverage</h2>
      <h3>{{ beverage.name }} {{ beverage.price }} THB</h3>
    </div>
    <div
      class="change"
      v-if="beverage"
    >
      <h2>Your Change</h2>
      <div class="coin">
        Coin 1 THB : <span>{{ change.one }}</span>
      </div>
      <div class="coin">
      Coin 2 THB : <span>{{ change.two }}</span>
      </div>
      <div class="coin">
      Coin 5 THB : <span>{{ change.five }}</span>
      </div>
      <div class="coin">
      Coin 10 THB : <span>{{ change.ten }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BeverageCard from '~/components/BeverageCard'
export default {
  components: {
    BeverageCard
  },
  data() {
    return {
      wallet: 0,
      beverages: null,
      total: null,
      change: {
        one: 0,
        two: 0,
        five: 0,
        ten: 0
      },
      beverage: null
    }
  },
  methods: {
    add(coin) {
      this.wallet = this.wallet + coin
    },
    selectItem(item) {
      if (!item.in_stock) {
        alert(`${item.name} is sold out.`)
        return
      }
      if (this.wallet >= item.price) {
        this.wallet = this.wallet - item.price
        this.getChange()
        this.beverage = item
      } else {
        alert(`Your wallet is not enough to buy ${item.name}.`)
      }
    },
    getChange() {
      this.change = {
        one: 0,
        two: 0,
        five: 0,
        ten: 0
      }
      while (this.wallet > 0) {
        if (this.wallet >= 10) {
          this.change.ten = this.change.ten + 1
          this.wallet = this.wallet - 10
        }
        if (this.wallet >= 5) {
          this.change.five = this.change.five + 1
          this.wallet = this.wallet - 5
        }
        if (this.wallet >= 2) {
          this.change.two = this.change.two + 1
          this.wallet = this.wallet - 2
        }
        if (this.wallet >= 1) {
          this.change.one = this.change.one + 1
          this.wallet = this.wallet - 1
        }
      }
    }
  },
  mounted() {
    axios.get('http://www.mocky.io/v2/5af11f8c3100004d0096c7ed').then(res => {
      this.beverages = res.data.data
      this.total = res.data.total
    })
  }
}
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  .coinInsertContainer {
    display: flex;
    align-items: center;
    margin-top: 20px;
    > .coin {
      border: 2px solid #aaa;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 0 10px;
      cursor: pointer;
    }
    > .coinOne {
      @extend .coin;
      width: 50px;
      height: 50px;
    }
    > .coinTwo {
      @extend .coin;
      width: 55px;
      height: 55px;
    }
    > .coinFive {
      @extend .coin;
      width: 65px;
      height: 65px;
    }
    > .coinTen {
      @extend .coin;
      width: 70px;
      height: 70px;
    }
  }
  .wallet {
    text-align: center;
    margin-top: 20px;
  }
  .beveragesContainer {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 20px;
    margin-top: 20px;
  }
  .change {
    margin-top: 20px;
    text-align: center;
    > div  {
      font-size: 1.2em;
      margin: 10px 0;
      > span {
        font-weight: bold;
      }
    }
  }
  .item {
    margin-top: 20px;
    text-align: center;
    > div  {
      font-size: 1.2em;
      margin: 10px 0;
    }
  }
}
</style>


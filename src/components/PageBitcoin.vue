<template>
  <div id="container">
    <h2 class="bg-gray-300 p-5 text-center my-5 shadow-4xl font-bold text-3xl" id="title">
      Bitcoin (BTC)
    </h2>
    <div class="items-center md:grid justify-center m-4" id="container">
      <div class="flex text-center justify-center font-bold text-5xl" id="container">
        <img class="w-12 h-14 " v-bind:src="image" />
       {{`${bitcoin},${Random}`}}
      </div>
      <div
        class="
        
          items-center
          justify-center
          bg-gray-300
          rounded-xl
          shadow-4xl
          m-4
          md:flex
        "
        id="container"
      >
        <form
          class="items-center  p-4 justify-center text-center"
          @submit.prevent="getHistory"
        >
        <span class="font-bold italic">Search bitcoin value by desired date!</span>
         <p class="flex text-center justify-center font-bold text-3xl p-3"></p>
          <span class="m-2 font-bold">Initial date/time:</span>
          <input class="border-black p-3 eft-px" step="00:15" type="datetime-local" id="dateTimeInitial" /><br>
          <span class="m-3.5 font-bold">End date/time:</span>
          <input class="border-black p-3 m-2" step="00:15" type="datetime-local" id="dateTimeEnd" />
          <br>
          <button
            class="
              m-4
              rounded-xl
              w-20
              h-10
              bg-black
              hover:bg-gray-900
              text-white
              cursor-pointer
            "
            type="submit"
          >
          Search
          </button>
        </form>
        <!--<p class="flex text-center justify-center font-bold text-3xl p-3">{{bitcoinHistory}}</p>-->
      </div>
    </div>
      <tr>
        <th class="w-1/2">Prices</th>
      </tr>
      <tr v-for=" value in bitcoinHistory" :key="value">
          {{qual(value)}}
        </tr>
  </div>
</template>
<script>
import api from "./../api";
import moment from "moment";
export default {
  data() {
    return {
      bitcoin: {},
      bitcoinHistory: "",
      dateTimeInitial: "",
      dateTimeEnd: "",
      image:
        "https://assets.coingecko.com/coins/images/1/thumb/bitcoin.png?1547033579",
    };
  },
  methods: {
    qual(e){
      console.log({e})
      return e
    },
     async getBitcoin() {
        const response = await api.get(
          "simple/price?ids=bitcoin&vs_currencies=brl"
        )
          this.bitcoin = response.data.bitcoin.brl.toLocaleString("pt-BR" ,{
            currency:"BRL"
          });
          this.getBitcoin();
    },
    async getHistory() {
      const dateTimeInitial = document.querySelector("#dateTimeInitial"); 
      const dateTimeEnd = document.querySelector("#dateTimeEnd"); 
      this.dateTimeInitial = moment(String(dateTimeInitial.value)).unix("DD-MM-YYYYTHH:mm:ss a");
      this.dateTimeEnd = moment(String(dateTimeEnd.value)).unix("DD-MM-YYYYTHH:mm:ss a")
      console.log(this.dateTimeInitial)
      console.log(this.dateTimeEnd)
      const response = await api.get(
        `/coins/bitcoin/market_chart/range?vs_currency=brl&from=${this.dateTimeInitial}&to=${this.dateTimeEnd}`
      );
      this.bitcoinHistory = response.data.prices.toLocaleString("pt-BR",{
        style: "currency",
        currency:"BRL"
      });
      console.log(this.bitcoinHistory.slice(23-34))
    },
  },
  computed: {
     Random(){
      const cent= Math.round(Math.random()*(9-0)+0);
      return `${cent}${cent}`
    }
  },
    mounted() {
    //this.getBitcoin();
  },
};
</script>
<style scoped>
</style>

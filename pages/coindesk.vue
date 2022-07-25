<template>
  <div>
    <v-container class="fill-height">
      <v-row align="center" justify="center">
        <v-col cols="12" sm="8" md="5">
          <v-card color="blue lighten-3">
            <v-container>
              <!-- <div> {{ info }}</div> -->
              <v-data-table
                :headers="headers"
                :items="info"
                :items-per-page="5"
                class="elevation-1"
              ></v-data-table>
              <!-- <v-card-title class="blue">Bitcoin Price Index</v-card-title>
             
              <div v-for="currency in info" class="currency">
                {{ currency.description }}<v-spacer/>{{ currency.code }}:
                 <span class="lighten">
      <strong v-html="currency.symbol"></s>{{ currency.rate_float | currencydecimal }}
    </span>
              </div> -->
            </v-container>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "coindesk",
  data() {
    return {
      page: 1,
      info: [],
      totalItems: null,
      currentPage: null,
      perPage: 5,
      
      itemsPerPage: 5,
      headers: [
        { text: "Code", value: "code" },
        { text: "Description", value: "description" },
        { text: "Currency", value: "symbol" },
        { text: "original Value", value: "rate" },
        // { text: "Value", value: "rate_float" },
      ],
    };
  },
  //   methods:{
  //     getinfo(){

  //         const URL="https://api.coindesk.com/v1/bpi/currentprice.json";
  //         axios.get(URL)
  //         .then(res =>{
  //             console.log(res.data.bpi);
  //         })
  //     }
  //   },

  filters: {
    currencydecimal(value) {
      return value.toFixed(2);
    },
  },
 created() {
    this.getCurrentPrice();
  },

   methods: {
    getCurrentPrice() {
      axios
      .get("https://api.coindesk.com/v1/bpi/currentprice.json")
      .then((response) => {
       
        this.info = Object.keys(response.data.bpi).map((key) => {
          return response.data.bpi[key]
        })

      }
      );
    }
   },
};
</script>

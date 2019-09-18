<template>
  <div class="home">
    <link
      rel="stylesheet"
      href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
      integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"
      crossorigin="anonymous"
    />
    <h1>Expense Report</h1>

    <div class="row">
      <div class="col-md-4">
        Description
        <input v-model="desc" type="text" name="descript" id="descript" />
      </div>
      <div class="col-md-4">
        Price
        <input v-model="price" type="text" name="price" id="price" />
      </div>
      <div class="col-md-4">
        <div class="input-group mb-3">
          <label>type the rate key in CAPS (HKD)</label>
          <input v-model="rateInput" type="text" name="rate" id="rate" />
          <div class="input-group-append">
            <button class="btn btn-primary" for="inputGroupSelect02" @click="conadd">ADD</button>
          </div>
          <button
            :disabled="totalCost > 1000"
            class="submit btn btn-secondary"
            @click="submitReceipts"
            id="submitreceipts"
          >submit</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  name: "home",
  components: {},
  data() {
    return {
      ratesValue: [],
      ratesKey: [],
      rates: {},
      price: 0,
      desc: "",
      arrayOfReceipts: [],
      rateInput: "",
      totalCost: 0
    };
  },
  mounted() {
    axios
      .get("https://api.exchangeratesapi.io/latest?base=CAD")
      .then(res => {
        // console.log(Object.keys(res.data.rates));
        // console.log(Object.values(res.data.rates));
        console.log(res.data.rates);
        this.ratesValue = Object.values(res.data.rates);
        this.ratesKey = Object.keys(res.data.rates);
        this.rates = res.data.rates;
      })
      .catch(e => {
        console.log(e);
      });
  },
  methods: {
    conadd() {
      let tempRate = this.rates;
      console.log(document.getElementById("rate").value);
      let test = this.ratesKey.indexOf(document.getElementById("rate").value);
      console.log(test);
      console.log(this.ratesValue[test]);
      let temp = {
        desc: this.desc,
        price: this.price,
        conv: this.ratesValue[test],
        convPrice: this.price / this.ratesValue[test]
      };
      console.log(temp);

      if (this.arrayOfReceipts.length < 5) this.arrayOfReceipts.push(temp);

      this.totalCost = 0;
      this.arrayOfReceipts.forEach(x => {
        this.totalCost += x.convPrice;
      });
      console.log(this.totalCost);
    },
    submitReceipts() {
      console.log(this.arrayOfReceipts.length);
      console.log(this.totalCost);
    }
  }
};
</script>

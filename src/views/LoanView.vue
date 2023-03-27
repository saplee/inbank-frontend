<template>
  <main>
    <div class="container">
      <form onsubmit="return false">
        <label><strong>Personal id:</strong></label>
        <input type="text" placeholder="Enter your personal id" v-model="personalCode" required>
        <label><strong>Loan amount: {{ loanAmount }}€</strong></label>
        <button id="minus" class="rangeButton" v-on:click="minus('loan amount')">-</button>
        <button id="plus" class="rangeButton" v-on:click="plus('loan amount')">+</button>
        <input type="range" class="range" placeholder="Enter loan amount" min="2000" max="10000" step="1"
               v-model="loanAmount">
        <label><strong>Months: {{ loanPeriod }}</strong></label>
        <button id="minus" class="rangeButton" v-on:click="minus()">-</button>
        <button id="plus" class="rangeButton" v-on:click="plus()">+</button>
        <input type="range" class="range" placeholder="Enter months" min="12" max="60" step="1" v-model="loanPeriod">
        <button v-on:click="post()" class="request">Calculate loan</button>
        <br>
        <br>
        <br>
        <br>
        <br>
        <p class="result1">Loan that we can give: {{ money }}€</p>
        <p class="result1" id="period">Loan period: {{ loanPeriod }} months</p>
        <p class="result2">Decision: {{ decision }}</p>
      </form>
    </div>
  </main>
</template>

<script>
import axios from "axios";
import {defineComponent} from "vue";

export default defineComponent({
  data: function () {
    return {
      personalCode: "",
      loanAmount: "",
      loanPeriod: "",
      money: null,
      newLoanPeriod: null,
      decision: null
    }
  },
  methods: {
    post: function () {
      let info = {
        personalCode: this.personalCode,
        loanAmount: this.loanAmount,
        loanPeriod: this.loanPeriod
      }
      if (info.personalCode.trim() !== "" &&
          info.loanAmount.trim() !== "" &&
          info.loanPeriod.trim() !== "") {
        axios.post("api/user/loan", info)
            .then(response => {
              this.money = response.data.loanAmount
              this.decision = response.data.decision
              if (parseInt(this.loanPeriod) !== parseInt(response.data.loanPeriod)) {
                document.getElementById("period").innerText = "New loan period:" + response.data.loanPeriod + " months"
              }
              else {
                document.getElementById("period").innerText = "Loan period:" + response.data.loanPeriod + " months"
              }
            })
            .catch(e => alert("Wrong id or your account is in debt!"))
      }
    },
    plus(type) {
      if (type === "loan amount") {
        this.loanAmount++
      } else {
        this.loanPeriod++
      }
    },
    minus: function (type) {
      if (type === "loan amount") {
        this.loanAmount--
      } else {
        this.loanPeriod--
      }
    }
  }
})

</script>

<style>


.container {
  left: 50%;
  top: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
  max-width: 30em;
  max-height: 100%;
}

input[type=range] {
  width: 100%;
}

input {
  width: 100%;
  padding: 1em 2em;
  margin: 0.5em 0;
}

button {
  width: 100%;
  font-size: 1em;
}

.range {
  padding: 1em 0;
}

.rangeButton {
  flex: 0 0 auto;
  width: 40px;
  height: 40px;
  border-radius: 100%;
  background: white;
  font-size: 24px;
  border: 1px solid lightgrey;
  cursor: pointer;
  -webkit-appearance: none;
  margin: 0 10px;
}

.result1 {
  text-align: center;
}

.result2 {
  text-align: center;
  top: 10%;
}

.request {
  background-color: #312054;
  color: #fff;
  border: none;
  border-radius: 10px;
  padding: 15px;
}


</style>
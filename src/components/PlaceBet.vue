<template>
  <div>
    <table>
      <tr>
        <td>
          <v-btn @click="setMin" :disabled="!disableButton">MIN</v-btn>
        </td>
        <td class="bet-input" rowspan="2">
          <span class="input-symbol">
            <input type="number" v-model="bet" :disabled="!disableButton" />
          </span>
        </td>
        <td>
          <v-btn @click="setBet(0.5)" :disabled="!disableButton">1/2</v-btn>
        </td>
      </tr>
      <tr>
        <td>
          <v-btn @click="setMax" :disabled="!disableButton">MAX</v-btn>
        </td>
        <td>
          <v-btn @click="setBet(2)" :disabled="!disableButton">2x</v-btn>
        </td>
      </tr>
      <tr>
        <td class="chip-row" colspan="3">
          <div class="chip-container">
            <v-btn
              class="chip"
              v-for="betValue in betValueRange"
              @click="addBet(betValue)"
              :key="betValue"
              :disabled="!disableButton"
            >
              +{{ betValue }}
            </v-btn>
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "PlaceBet",
  data() {
    return {
      bet: 10,
      betValueRange: [5, 10, 50, 100, 500, 1000],
    };
  },
  props: {
    disableButton: Boolean,
  },
  methods: {
    setMin() {
      this.bet = 10;
    },
    setMax() {
      this.bet = 3000;
    },
    setBet(rate: number) {
      this.bet = Math.round(this.bet * rate);
    },
    addBet(betValue: number) {
      this.bet += betValue;
    },
  },
  watch: {
    bet: {
      deep: true,
      handler(val) {
        /**
         * Ensure that the bet value does not exceed the maximum threshold
         */
        if (val > 3000) {
          this.bet = 3000;
        }

        this.$emit("updateBetValue", this.bet);
      },
    },
  },
});
</script>

<style lang="less" scoped>
@media screen and (max-width: 600px) {
  table {
    width: 100% !important;
  }
}

table {
  border: 1px solid #293a53;
  border-radius: 10px;
  color: white;
  background-color: #1c2234;
  width: 100%;
  max-width: 100%;
  padding: 5px;
}

tr,
td {
  text-align: center;
  padding: 5px;
}

.chip-row {
  border-top: 1px solid #293a53;
}
.chip-container {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;

  .chip {
    width: 50px;
    cursor: pointer;
    padding: 5px;
  }
}

.bet-input {
  width: 300px;

  .input-symbol {
    position: relative;

    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }

    &:before {
      position: absolute;
      top: -36px;
      font-size: 47px;
      content: "¥";
      left: 20px;
      font-weight: bold;
    }

    input[type="number"] {
      moz-apperance: textfield;
      font-size: 47px;
      width: 100%;
      text-align: center;
      color: white;
      font-weight: bold;

      &:focus {
        outline: none;
      }
    }
  }
}

.v-btn {
  background-color: #21354e !important;
  color: #adc1db !important;
}
</style>

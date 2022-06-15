<template>
  <v-container class="betting-container">
    <v-row>
      <v-col cols="12" md="6" lg="6" sm="12">
        <place-bet @updateBetValue="updateBet" :disable-button="betStatus" />
      </v-col>
      <v-col cols="12" md="4" lg="3" sm="12">
        <defuse-bet @defuseUpdate="updateDefuse" :disable-button="betStatus" />
      </v-col>
      <v-col cols="12" md="2" lg="3" sm="12">
        <v-btn
          class="place-bet bet-unplaced"
          v-if="betStatus"
          @click="placeBet"
        >
          BET
        </v-btn>
        <v-btn disabled class="place-bet bet-placed" v-if="!betStatus">
          BET<br />PLACED
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
import PlaceBet from "@/components/PlaceBet.vue";
import DefuseBet from "@/components/DefuseBet.vue";

export default Vue.extend({
  name: "BettingView",
  components: {
    PlaceBet,
    DefuseBet,
  },
  data() {
    return {
      bet: 10,
      defuseValue: 1,
      status: "bet-unplaced",
    };
  },
  methods: {
    placeBet() {
      this.status = "bet-placed";
    },
    updateBet(data: number) {
      this.bet = data;
    },
    updateDefuse(data: number) {
      this.defuseValue = data;
    },
  },
  computed: {
    betStatus(): boolean {
      return this.status == "bet-unplaced";
    },
  },
});
</script>

<style lang="less">
@media screen and (max-width: 959px) {
  .place-bet {
    height: 100px !important;
  }
}

.betting-container {
  font-family: "Oxanium", cursive !important;
}

.v-btn:not(.v-btn--round).v-size--default.place-bet {
  height: 100%;
}

.place-bet {
  width: 100%;
}

.bet-unplaced {
  background-image: linear-gradient(130deg, #2182d8, #1dbadd);
  color: white !important;
  font-size: 22px !important;
  font-weight: bold;
}

.bet-placed {
  background-image: linear-gradient(130deg, #1e2f4a, #1c3a4b);
  font-weight: bold;
  font-size: 22px !important;
}

.theme--light.v-btn.v-btn--disabled {
  color: #1eb9da !important;
}
</style>

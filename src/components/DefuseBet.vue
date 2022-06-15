<template>
  <div class="defuse-container">
    <div v-if="!isMobile">
      <div class="defuse-header">DEFUSE AT</div>
      <div class="defuse-val-container">
        <v-btn @click="updateDefuseValue('-')" :disabled="!disableButton">
          -
        </v-btn>
        <div class="defuse-value">{{ defuseDisplay }}</div>
        <v-btn @click="updateDefuseValue('+')" :disabled="!disableButton">
          +
        </v-btn>
      </div>
    </div>
    <div v-if="isMobile" class="defuse-mobile">
      <div class="defuse-header">DEFUSE AT</div>
      <div class="defuse-val-container">
        <v-btn @click="updateDefuseValue('-')" :disabled="!disableButton">
          -
        </v-btn>
        <div class="defuse-value">{{ defuseDisplay }}</div>
        <v-btn @click="updateDefuseValue('+')" :disabled="!disableButton">
          +
        </v-btn>
      </div>
    </div>
    <div class="defuse-multiplier" v-if="!isMobile">
      <v-btn
        class="multipler"
        v-for="multi in multiplier"
        :key="multi"
        @click="defuseValueMultiplier(multi)"
        :disabled="!disableButton"
      >
        {{ multi }}x
      </v-btn>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "DefuseBet",
  data() {
    return {
      multiplier: [1.5, 2.0, 5.0],
      defuseValue: 1,
      innerWidth: window.innerWidth,
    };
  },
  props: {
    disableButton: Boolean,
  },
  methods: {
    updateDefuseValue(operator: string) {
      if (operator == "+") {
        if (this.defuseValue < 2) {
          this.defuseValue += 0.1;
        } else {
          this.defuseValue += 1;
        }
      } else {
        if (this.defuseValue <= 2 && this.defuseValue >= 0.1) {
          this.defuseValue -= 0.1;
        } else if (this.defuseValue > 2) {
          this.defuseValue -= 1;
        }
      }
    },
    defuseValueMultiplier(multiplier: number) {
      this.defuseValue *= multiplier;
    },
    resizeHandler(e) {
      console.log(e);
      this.innerWidth = e.currentTarget.innerWidth;
    },
  },
  computed: {
    defuseDisplay(): string {
      return this.defuseValue.toFixed(2);
    },
    isMobile(): boolean {
      return this.innerWidth < 960;
    },
  },
  watch: {
    defuseValue: {
      deep: true,
      handler(val) {
        this.$emit("defuseUpdate", val);
      },
    },
  },
  created() {
    window.addEventListener("resize", this.resizeHandler);
  },
  destroyed() {
    window.removeEventListener("resize", this.resizeHandler);
  },
});
</script>

<style lang="less" scoped>
.defuse-container {
  width: 100%;
  background-color: #1c2234;
  border: 1px solid #293a53;
  border-radius: 10px;

  .defuse-mobile {
    display: flex;
    flex-direction: row;
    padding: 10px;

    .defuse-header {
      margin-top: 8px;
      width: 100%;
      text-align: unset;
    }

    .defuse-val-container {
      justify-content: flex-end;
      width: 100%;
      gap: 10px;
    }
  }

  .v-btn {
    background-color: #21354e !important;
    color: #adc1db !important;
  }

  .defuse-header {
    width: inherit;
    text-align: center;
    margin-top: 20px;
    color: #637286;
  }

  .defuse-val-container {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;

    .defuse-value {
      color: white;
      font-weight: bold;
      font-size: 25px;
    }
  }

  .defuse-multiplier {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    margin-top: 24px;
    padding: 8px 0;
    border-top: 1px solid #293a53;
  }
}
</style>

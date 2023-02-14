<template>
  <v-container>
    <v-card elevation="0">
      <v-card-title class="grey--text text-subtitle-2 ps-0">
        Board Equipment
      </v-card-title>
      <v-divider></v-divider>

      <table class="tableexhaust text-center ma-6">
        <tr>
          <td class="ps-2 pe-2">Number of Main Key:</td>
          <td class="ps-2 pe-2">Number of Contactor:</td>
          <td class="ps-2 pe-2">Number of Thermal Key:</td>
          <td class="ps-2 pe-2">Number of Miniature Key:</td>
          <td class="ps-2 pe-2">Number of Inverter:</td>
        </tr>

        <tr>
          <td class="insertdata">
            <input type="number" id="MainKey" :value="MainKey" />
          </td>
          <td class="insertdata">
            <input type="number" id="Contactor" :value="totalContactorsNum" />
          </td>
          <td class="insertdata">
            <input type="number" id="ThermalKey" :value="totalThermalKeyNum" />
          </td>
          <td class="insertdata">
            <input type="number" id="MiniatureKey" :value="sendMiniatureKey" />
          </td>

          <td class="insertdata">
            <input type="number" id="Inverter" :value="totalInverterNum" />
          </td>
        </tr>

        <tr>
          <td class="ps-2 pe-2">Number of relay:</td>
          <td class="ps-2 pe-2">Number of AI:</td>
          <td class="ps-2 pe-2">Number of AO:</td>
          <td class="ps-2 pe-2">Number of DI:</td>
          <td class="ps-2 pe-2">Number of DO:</td>
        </tr>

        <tr>
          <td class="insertdata">
            <input type="number" id="relay" :value="totalRelayNum" />
          </td>
          <td class="insertdata">
            <input type="number" id="AI" :value="totalSensorNumbers" />
          </td>
          <td class="insertdata">
            <input type="number" id="AO" :value="AO" />
          </td>
          <td class="insertdata">
            <input type="number" id="DI" :value="totalDI" />
          </td>
          <td class="insertdata">
            <input type="number" id="DO" :value="totalDO" />
          </td>
        </tr>
      </table>
    </v-card>
    <v-card elevation="0">
      <v-card-title class="grey--text text-subtitle-2 ps-0">
        Applied Current
      </v-card-title>
      <v-divider></v-divider>
      <table class="tableexhaust text-center ma-6">
        <tr>
          <td class="ps-2 pe-2">Total Power Consumption of Fans:</td>
          <td class="ps-2 pe-2">Factor:</td>
          <td class="ps-2 pe-2">Applied Current:</td>
          <td class="ps-2 pe-2">Current List:</td>
        </tr>
        <tr>
          <td class="insertdata">
            <input type="number" id="TotalPower" :value="totalPower" />
          </td>
          <td class="insertdata">
            <input type="number" id="Factor" :value="factor" />
          </td>
          <td class="insertdata">
            <input
              type="number"
              id="AppliedCurrent"
              v-model="toFixed2AppliedCurrent"
            />
          </td>
          <td class="insertdata">
            <input type="number" id="CurrentList" :value="CurrentList" />
          </td>
        </tr>
      </table>
    </v-card>
    <v-btn
      color="primary"
      class="ma-6"
      elevation="3"
      @click="ProgrammingCostVisibility()"
      >Submit</v-btn
    >
    <programmingCost v-if="ShowProgrammingCost" />
  </v-container>
</template>

<script>
import ProgrammingCost from "./ProgrammingCost.vue";

export default {
  components: { ProgrammingCost },
  data() {
    return {
      MiniatureKey: null,
      FreshAirContactor: null,
      FreshAirThermalKey: null,
      MainKey: 1,
      AO: 0,
      factor: 2.2,
      ShowProgrammingCost: false,
      powerConsumptionOfFreshAirFans: null,
      powerConsumptionOfExhaustFans: null,
      PowerConsumptionOfJetFans: null,
      totalPower: null,
      totalSensorNumbers: null,
      AppliedCurrent: null,
      CurrentList: null,
      toFixed2AppliedCurrent: null,
    };
  },

  props: [
    "zonesNumber",
    "sendMiniatureKey",
    "totalInverterNum",
    "totalThermalKeyNum",
    "totalRelayNum",
    "totalContactorsNum",
    "totalDI",
    "totalDO",
  ],
  methods: {
    ProgrammingCostVisibility() {
      this.ShowProgrammingCost = true;
    },
    CurrentListMethod() {
      // this.CurrentList = 0;
      if (0 < this.toFixed2AppliedCurrent && this.toFixed2AppliedCurrent <= 2) {
        console.log("avali");
        this.CurrentList = 2;
      } else if (
        2 < +this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 4
      ) {
        console.log("dovomi");
        this.CurrentList = 4;
      } else if (
        4 < +this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 6
      ) {
        this.CurrentList = 6;
      } else if (
        6 < this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 10
      ) {
        this.CurrentList = 10;
      } else if (
        10 < this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 16
      ) {
        this.CurrentList = 16;
      } else if (
        16 < this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 20
      ) {
        this.CurrentList = 20;
      } else if (
        20 < this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 25
      ) {
        this.CurrentList = 25;
      } else if (
        25 < this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 32
      ) {
        this.CurrentList = 32;
      } else if (
        32 < this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 40
      ) {
        this.CurrentList = 40;
      } else if (
        40 < this.toFixed2AppliedCurrent &&
        this.toFixed2AppliedCurrent <= 50
      ) {
        this.CurrentList = 50;
      } else if (
        (50 < this.toFixed2AppliedCurrent &&
          this.toFixed2AppliedCurrent <= 63) ||
        63 < this.toFixed2AppliedCurrent
      ) {
        this.CurrentList = 63;
      }
    },
  },
  computed: {
    getMiniatureKey() {
      this.MiniatureKey = +this.$store.state.Product.MiniatureKey;
      console.log(MiniatureKey, "MiniatureKey");
      return this.MiniatureKey;
    },
    getTotalFreshAirPower() {
      this.powerConsumptionOfFreshAirFans =
        this.$store.state.Product.totalFreshAirPower;
      return this.powerConsumptionOfFreshAirFans;
    },
    getTotalExhaustPower() {
      this.powerConsumptionOfExhaustFans =
        this.$store.state.Product.totalexhaustPower;
      return this.powerConsumptionOfExhaustFans;
    },
    getTotalJetFanPower() {
      this.PowerConsumptionOfJetFans =
        this.$store.state.Product.totalJetFanPower;
      return this.PowerConsumptionOfJetFans;
    },
    getTotalSensorNumber() {
      this.totalSensorNumbers = this.$store.state.Product.totalSensorNumber;
      return this.totalSensorNumbers;
    },
    totalPowerConsumption() {
      this.totalPower =
        +this.powerConsumptionOfFreshAirFans +
        +this.powerConsumptionOfExhaustFans +
        +this.PowerConsumptionOfJetFans;
    },
    AppliedCurrentCalculation() {
      this.AppliedCurrent = this.totalPower * this.factor;
      this.toFixed2AppliedCurrent = this.AppliedCurrent.toFixed(2);
    },
  },
  updated() {
    this.getTotalFreshAirPower;
    this.getTotalExhaustPower;
    this.getTotalJetFanPower;
    this.totalPowerConsumption;
    this.getTotalSensorNumber;
    this.AppliedCurrentCalculation;
    this.CurrentListMethod();
  },
};
</script>
<style scoped>
.border {
  border: 1px solid black;
}
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
  border-color: #bdbdbd;
}
th.hidden {
  border-style: none;
}
.insertdata {
  background: #f5fbff;
}
.insertdata input {
  width: 90%;
  text-align: center;
  outline: none;
  border: 1px solid #bbdefb;
  background: white;
}
.insertdata input:focus {
  background: rgb(255, 255, 238);
}
.headertable {
  width: 20%;
  border-bottom: 1px solid #eeeeee;
}
th {
  font-weight: initial;
  font-size: 0.8rem;
}
.bordertable {
  width: 100%;
  padding-top: 0.4rem;
  border: none;
}
.tableexhaust td,
.tableexhaustshaft td,
.tablesupplyt td,
.tablesupplyshaft td,
.tablejetfans td,
.tablecosensors td {
  font-size: 0.7rem;
}

#hasSensor {
  padding: 1px 20px;
  background: #fff;
  border: 1px solid #bbdefb;
}
input[disabled] {
  background: #e5e5e5;
}

.currentListSelectBox {
  min-width: 80px;
  text-align: center;
  background: #fff;
  margin: 0 10px;
  border: 1px solid #bbdefb;
}
</style>
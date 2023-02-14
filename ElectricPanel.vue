<template>
  <v-container>
    <v-card elevation="0">
      <v-card-title class="grey--text text-subtitle-2 ps-0"
        >Electric Panels</v-card-title
      >
      <v-divider></v-divider>
      <v-card-title class="headertable grey--text text-subtitle-2 ps-0"
        >Please fill the table</v-card-title
      >

      <table class="tableexhaust text-center ma-6">
        <tr>
          <td class="ps-2 pe-2">The Length (m)</td>
          <td class="ps-2 pe-2">The Width (m)</td>
          <td class="ps-2 pe-2">Area (m<sup>2</sup>)</td>
          <td class="ps-2 pe-2">Does the project have sensors?</td>
        </tr>
        <tr>
          <td class="insertdata">
            <input
              type="number"
              id="length"
              name="length"
              value="40"
              v-model="length"
              @change="calculateArea"
            />
          </td>

          <td class="insertdata">
            <input
              type="number"
              id="width"
              name="width"
              value="20"
              v-model="width"
              @change="calculateArea"
            />
          </td>
          <td class="insertdata">
            <input
              type="number"
              id="area"
              name="area"
              value="0"
              v-model="area"
            />
          </td>
          <td class="insertdata">
            <input type="checkbox" v-model="hasSensor" />
          </td>
        </tr>
      </table>
    </v-card>

    <HasSensorTable
      :disableInputs="hasSensor"
      :length="length"
      :width="width"
    />

    <table class="tableexhaust text-center ma-6">
      <tr>
        <td class="ps-2 pe-2">Number of Floors:</td>
        <td class="ps-2 pe-2">Number of Zones:</td>
        <!-- <td class="ps-2 pe-2">Number of Fresh Air Fan:</td>
        <td class="ps-2 pe-2">Number of Exhaust Fan:</td> -->
        <td class="ps-2 pe-2">Number of Smoke Curtain:</td>
        <td class="ps-2 pe-2">Number of Damper Supply:</td>
        <td class="ps-2 pe-2">Number of Damper Exhaust:</td>
        <!-- <td class="ps-2 pe-2">Number of Inverter:</td> -->
      </tr>
      <tr>
        <td class="insertdata">
          <input
            type="number"
            id="floorsNumber"
            v-model="floorsNumber"
            @change="calculationOfCurtainDamperExhaust()"
          />
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="zonesNumber"
            v-model="zonesNumber"
            @change="calculationOfCurtainDamperExhaust()"
          />
        </td>

        <!-- <td class="insertdata">
          <input type="number" id="freshAirFan" v-model="freshAirFan" />
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="ExhaustFan"
            v-model="ExhaustFan"
            @change="calculationOfCurtainDamperExhaust()"
          />
        </td> -->
        <td class="insertdata">
          <input type="number" id="smokeCurtain" v-model="smokeCurtain" />
        </td>
        <td class="insertdata">
          <input type="number" id="DamperSupply" v-model="DamperSupply" />
        </td>
        <td class="insertdata">
          <input type="number" id="DamperExhaust" v-model="DamperExhaust" />
        </td>
        <!-- <td class="insertdata">
          <input type="number" id="Inverter" v-model="Inverter" />
        </td> -->
      </tr>

      <tr>
        <td class="ps-2 pe-2">Does it have HMI?</td>
        <td class="ps-2 pe-2">
          If it has HMI, the below percentage <br />
          will be added to the total price
        </td>
        <td class="ps-2 pe-2">
          DI unit programming cost <br />
          (Hezar Tooman):
        </td>
        <td class="ps-2 pe-2">
          AI unit programming cost <br />
          (Hezar Tooman):
        </td>
        <td class="ps-2 pe-2">
          The below percentage <br />
          will be added to the total price
        </td>
      </tr>
      <tr>
        <td class="insertdata">
          <input type="checkbox" v-model="hasHMI" @change="hasHMIfunc()" />
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="HMIPercent"
            v-model="HMIPercent"
            :disabled="!HMIPercent"
          />
        </td>
        <td class="insertdata">
          <input type="number" id="DIProgramming" v-model="DIProgramming" />
        </td>
        <td class="insertdata">
          <input type="number" id="AIProgramming" v-model="AIProgramming" />
        </td>
        <td class="insertdata">
          <input type="number" id="alak" v-model="alak" />
        </td>
      </tr>
    </table>

    <FloorTable
      :zonesNumber="zonesNumber"
      :smokeCurtain="smokeCurtain"
      :DamperSupply="DamperSupply"
      :DamperExhaust="DamperExhaust"
      :totalRelayNum="totalRelayNum"
      :hasHMI="hasHMI"
      :maxOfDamperSupplyDamperExhaust="maxOfDamperSupplyDamperExhaust"
      :dICurtain="dICurtain"
      :dIAlarm="dIAlarm"
      :floorsNumber="floorsNumber"
    />
    {{ DIParams }}
  </v-container>
</template>


<script>
import HasSensorTable from "./HasSensorTable.vue";
import FloorTable from "./FloorTable.vue";

export default {
  data() {
    return {
      length: null,
      width: 20,
      area: null,
      selected: "",
      hasSensor: false,
      floorsNumber: 1,
      zonesNumber: 1,
      freshAirFan: 0,
      ExhaustFan: 0,
      smokeCurtain: 0,
      DamperSupply: 0,
      DamperExhaust: 0,
      Inverter: 0,
      hasHMI: false,
      HMIPercent: null,
      DIProgramming: null,
      AIProgramming: null,
      alak: null,
      totalRelayNum: null,
      maxOfDamperSupplyDamperExhaust: null,
      dICurtain: null,
      dIAlarm: null,
    };
  },
  methods: {
    calculateArea(event) {
      this.area = this.length * this.width;
      this.freshAirFan = 2;
      this.Inverter = 0;
      this.DIProgramming = 200;
      this.AIProgramming = 350;
      this.alak = 4;
      this.$store.commit("sendDIProgramming", this.DIProgramming);
      this.$store.commit("sendAIProgramming", this.AIProgramming);
    },

    hasHMIfunc() {
      if (this.hasHMI) {
        this.HMIPercent = 50;
      } else {
        this.HMIPercent = null;
      }
    },
    calculationOfCurtainDamperExhaust() {
      if (this.zonesNumber == 0) {
        this.smokeCurtain = 0;
      } else {
        this.smokeCurtain = (+this.zonesNumber - 1) * +this.floorsNumber;
      }
      this.DamperSupply = +this.floorsNumber * +this.zonesNumber;
      this.DamperExhaust = +this.floorsNumber * +this.zonesNumber;
      this.ExhaustFan = +this.floorsNumber * +this.zonesNumber;
      this.totalRelayNum =
        this.smokeCurtain + this.DamperSupply + this.DamperExhaust;
    },
  },
  computed: {
    DIParams() {
      this.maxOfDamperSupplyDamperExhaust = Math.max(
        this.DamperSupply,
        this.DamperExhaust
      );
      this.dICurtain = this.floorsNumber * (this.zonesNumber - 1);
      this.dIAlarm = this.floorsNumber * this.zonesNumber;
    },
  },

  components: { HasSensorTable, FloorTable },
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
</style>
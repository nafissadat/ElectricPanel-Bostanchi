<template>
  <div>
    <v-btn color="primary" class="ma-6" elevation="3" @click="rowObject()"
      >Submit</v-btn
    >
    <table class="tableexhaust text-center ma-6" v-if="showFloorTable">
      <tr>
        <td class="ps-2 pe-2">Zone:</td>
        <td colspan="3" class="ps-2 pe-2">Fresh Air</td>
        <td colspan="3" class="ps-2 pe-2">Exhaust</td>
        <td colspan="2" class="ps-2 pe-2">Jet Fan:</td>
      </tr>
      <tr>
        <td class="ps-2 pe-2"></td>
        <td class="ps-2 pe-2">Number:</td>
        <td class="ps-2 pe-2">Power :</td>
        <td class="ps-2 pe-2">Number of Inverter:</td>
        <td class="ps-2 pe-2">Number:</td>
        <td class="ps-2 pe-2">Power :</td>
        <td class="ps-2 pe-2">Number of Inverter:</td>
        <td class="ps-2 pe-2">Number:</td>
        <td class="ps-2 pe-2">Power :</td>
      </tr>
      <tr v-for="(zone, index) in rowArray" :key="index">
        <td class="insertdata">{{ index + 1 }}</td>
        <td class="insertdata">
          <input type="number" id="freshAirNum" v-model="zone.freshAirNum" />
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="freshAirPower"
            v-model="zone.freshAirPower"
          />
        </td>
        <td class="insertdata">
          {{ claculateFreshAirInverter(index) }}
          <input
            type="number"
            id="freshAirInverter"
            v-model="zone.freshAirInverter"
          />
        </td>
        <td class="insertdata">
          <input type="number" id="exhaustNum" v-model="zone.exhaustNum" />
        </td>
        <td class="insertdata">
          <input type="number" id="exhaustPower" v-model="zone.exhaustPower" />
        </td>
        <td class="insertdata">
          {{ claculateExhaustInverter(index) }}
          <input
            type="number"
            id="exhaustInverter"
            v-model="zone.exhaustInverter"
          />
        </td>

        <td class="insertdata">
          <input type="number" id="jetFanNum" v-model="zone.jetFanNum" />
        </td>
        <td class="insertdata">
          <input type="number" id="jetFanPower" v-model="zone.jetFanPower" />
        </td>
      </tr>
      <tr>
        <td class="ps-2 pe-2">Sum</td>
        <td class="ps-2 pe-2">{{ freshAirSum }}</td>
        <td class="ps-2 pe-2">{{ freshAirPowerSum }}</td>
        <td class="ps-2 pe-2">{{ freshAirInverterSum }}</td>
        <td class="ps-2 pe-2">{{ exhaustSum }}</td>
        <td class="ps-2 pe-2">{{ exhaustPowerSum }}</td>
        <td class="ps-2 pe-2">{{ exhaustInverterSum }}</td>
        <td class="ps-2 pe-2">{{ jetFanSum }}</td>
        <td class="ps-2 pe-2">{{ jetFanPowerSum }}</td>
      </tr>
    </table>

    {{ totalFreshAirPower }}
    <v-btn
      color="primary"
      class="ma-6"
      elevation="3"
      @click="sendDataToStore()"
      v-if="showFloorTable"
      >Submit</v-btn
    >
    <BoardEquipmentTable
      v-if="ShowBoardEquipmentTable"
      :sendMiniatureKey="MiniatureKey"
      :totalInverterNum="totalInverterNum"
      :totalThermalKeyNum="totalThermalKeyNum"
      :totalRelayNum="totalRelayNum"
      :totalContactorsNum="totalContactorsNum"
      :totalDI="totalDI"
      :totalDO="totalDO"
    ></BoardEquipmentTable>
    {{ getRowArrayFromStore }}
  </div>
</template>


<script>
import BoardEquipmentTable from "./BoardEquipmentTable.vue";
export default {
  data() {
    return {
      showFloorTable: false,
      rowArray: [],
      floorTable: false,
      freshAirNumArray: [],
      totalRowArrayData: null,
      jetFanSumCalculate: null,
      MiniatureKey: null,
      sumOfFreshAirNumItems: null,
      MiniaturKeyForjetFans: null,
      ExhaustNumArray: [],
      sumOfExhaustNumItems: null,
      HasSmokeCurtain: null,
      HasDamper: null,
      totalInverterNum: null,
      thermalKeyFreshAirArray: [],
      sumOfFreshAirThermalKey: null,
      totalThermalKeyNum: null,
      thermalKeyExhaustArray: [],
      sumOfExhaustThermalKey: null,
      contactorFreshAirArray: [],
      sumOfFreshAirContactor: null,
      contactorExhaustArray: [],
      sumOfExhaustContactor: null,
      totalContactorsNum: null,
      ShowBoardEquipmentTable: false,
      handAutokey: 0,
      controlPhaseKey: 1,
      exhaustDI: 1,
      ExhaustFanHasInverter: false,
      ExhaustNumArrayDI: [],
      sumOfExhaustNumDI: null,
      freshAirDI: 1,
      freshAirNumArrayDI: [],
      sumOfFreshAirNumArrayDI: null,
      totalDI: null,
      totalDO: null,

      totalFreshAirPower: null,
    };
  },
  updated() {
    this.getRowArrayFromStore;
  },

  methods: {
    rowObject() {
      this.showFloorTable = true;
      this.rowArray = [];
      for (let index = 0; index < +this.zonesNumber; index++) {
        this.rowArray.push({
          freshAirNum: +this.$store.state.Product.ElectricPanelData.freshAirNum,
          freshAirPower:
            +this.$store.state.Product.ElectricPanelData.freshAirPower,
          freshAirInverter:
            +this.$store.state.Product.ElectricPanelData.freshAirInverter,
          exhaustNum: +this.$store.state.Product.ElectricPanelData.exhaustNum,
          exhaustPower:
            +this.$store.state.Product.ElectricPanelData.exhaustPower,
          exhaustInverter:
            +this.$store.state.Product.ElectricPanelData.exhaustInverter,
          jetFanNum: +this.$store.state.Product.ElectricPanelData.jetFanNum,
          jetFanPower: +this.$store.state.Product.ElectricPanelData.jetFanPower,
        });
      }
    },
    sendDataToStore() {
     
      
      this.ShowBoardEquipmentTable = true;
      setTimeout(() => {
        this.ThermalKeyFreshAirArrayReduce;
      }, 210);
      setTimeout(() => {
        this.ThermalKeyExhaustArrayReduce;
      }, 210);
      setTimeout(() => {
        this.ContactorExhaustReduce;
      }, 210);
      setTimeout(() => {
        this.ContactorFreshAirReduce;
      }, 210);

      setTimeout(() => {
        this.FreshAirMiniatureKey();
      }, 200);

      setTimeout(() => {
        this.freshAirNumSumItems;
      }, 200);

      setTimeout(() => {
        this.sumOfExhaustNumItems;
      }, 200);

      this.freshAirNumArray;
      this.ExhaustNumArray;
      this.ExhaustNumSumItems;

      this.HasSmokeCurtainMethod();
      this.HasDamperMethod();
      setTimeout(() => {
        this.ExhaustMiniatureKey();
      }, 200);
      setTimeout(() => {
        this.NumberofMiniatureKey();
      }, 200);
      setTimeout(() => {
        this.totalNumberOfInverter();
      }, 200);
      setTimeout(() => {
        this.totalNumberOfThermalKey();
      }, 215);
      setTimeout(() => {
        this.TotalNumberOfContactors();
      }, 215);
      setTimeout(() => {
        this.DIFunc();
      }, 230);
      setTimeout(() => {
        this.ExhaustNumArrayDIReduce;
      }, 215);
      setTimeout(() => {
        this.FrshAirNumArrayDIReduce;
      }, 215);
      setTimeout(() => {
        this.exhaustPowerSum;
      }, 300);

      // setTimeout(() => {
      //   this.freshAirPowerSum
      // }, 245);
    },

    FreshAirMiniatureKey() {
      this.freshAirNumArray.length = 0;
      this.thermalKeyFreshAirArray.length = 0;
      this.contactorFreshAirArray.length = 0;
      for (let length = 0; length < +this.zonesNumber; length++) {
        if (this.totalRowArrayData[length].freshAirPower > 7.5) {
          this.freshAirNumArray.push(
            +this.totalRowArrayData[length].freshAirNum
          );
        } else {
          this.thermalKeyFreshAirArray.push(
            +this.totalRowArrayData[length].freshAirNum
          );
          this.contactorFreshAirArray.push(
            +this.totalRowArrayData[length].freshAirNum
          );
        }
      }
    },

    ExhaustMiniatureKey() {
      this.ExhaustNumArray.length = 0;
      this.thermalKeyExhaustArray.length = 0;
      this.contactorExhaustArray.length = 0;
      for (let length = 0; length < +this.zonesNumber; length++) {
        if (this.totalRowArrayData[length].exhaustPower > 7.5) {
          this.ExhaustNumArray.push(+this.totalRowArrayData[length].exhaustNum);
        } else {
          this.thermalKeyExhaustArray.push(
            +this.totalRowArrayData[length].exhaustNum
          );
          this.contactorExhaustArray.push(
            +this.totalRowArrayData[length].exhaustNum
          );
        }
      }
    },
    HasSmokeCurtainMethod() {
      if (this.smokeCurtain != 0) {
        this.HasSmokeCurtain = 1;
      } else {
        this.HasSmokeCurtain = 0;
      }
    },
    HasDamperMethod() {
      if (this.DamperSupply != 0 || this.DamperExhaust != 0) {
        this.HasDamper = 1;
      } else {
        this.HasDamper = 0;
      }
    },

    NumberofMiniatureKey() {
      (this.MiniaturKeyForjetFans = this.jetFanSumCalculate),
        (this.MiniatureKey =
          +this.sumOfFreshAirNumItems +
          +this.MiniaturKeyForjetFans +
          +this.HasSmokeCurtain +
          +this.HasDamper +
          +this.ExhaustNumSumItems);
      return MiniatureKey;
    },
    totalNumberOfInverter() {
      this.totalInverterNum =
        this.freshAirInverterSum + this.exhaustInverterSum;
    },
    totalNumberOfThermalKey() {
      this.totalThermalKeyNum =
        this.sumOfFreshAirThermalKey +
        this.sumOfExhaustThermalKey +
        +this.jetFanSumCalculate;
    },
    TotalNumberOfContactors() {
      this.totalContactorsNum =
        this.sumOfFreshAirContactor +
        this.sumOfExhaustContactor +
        this.jetFanSumCalculate * 3;
    },
    /********************************************************************** */
    DIFunc() {
      this.ExhaustNumArrayDI.length = 0;
      if (this.hasHMI) {
        this.handAutokey = 1;
      }
      this.totalDI =
        this.handAutokey +
        this.controlPhaseKey +
        this.sumOfFreshAirNumArrayDI +
        this.sumOfExhaustNumDI +
        this.maxOfDamperSupplyDamperExhaust +
        this.dICurtain +
        this.dIAlarm;
      this.floorsNumber * this.jetFanSumCalculate * 2;
      this.totalDO =
        this.sumOfFreshAirNumArrayDI +
        this.sumOfExhaustNumDI +
        this.maxOfDamperSupplyDamperExhaust +
        this.dICurtain;
      this.floorsNumber * this.jetFanSumCalculate * 2;
      this.$store.commit("sendTotalDI", this.totalDI);
      this.$store.commit("sendTotalDO", this.totalDO);
    },
  },
  computed: {
    claculateFreshAirInverter() {
      return function (index) {
        this.rowArray[index].freshAirInverter = Math.floor(
          +this.rowArray[index].freshAirPower / 7.5
        );
      };
    },
    claculateExhaustInverter() {
      return function (index) {
        this.rowArray[index].exhaustInverter = Math.floor(
          +this.rowArray[index].exhaustPower / 7.5
        );
      };
    },

    freshAirSum() {
      this.freshAirNumSum1 = this.rowArray.reduce(
        (accumulate, current) => accumulate + +current.freshAirNum,
        0
      );
      this.$store.commit("sendfreshAirNumSum1", this.freshAirNumSum1);
      return this.freshAirNumSum1;
    },

    freshAirPowerSum() {
      let totalFreshAirPower = 0;
      for (let index = 0; index < this.rowArray.length; index++) {
        totalFreshAirPower +=
          this.rowArray[index].freshAirPower * this.rowArray[index].freshAirNum;
      }
      this.$store.commit("sendTotalFreshAirPower", totalFreshAirPower);
      return totalFreshAirPower;
    },
    freshAirInverterSum() {
      return this.rowArray.reduce(
        (accumulate, current) => accumulate + +current.freshAirInverter,
        0
      );
    },
    exhaustSum() {
      return this.rowArray.reduce(
        (accumulate, current) => accumulate + +current.exhaustNum,
        0
      );
    },
    exhaustPowerSum() {
      let totalexhaustPower = 0;
      for (let index = 0; index < this.rowArray.length; index++) {
        totalexhaustPower +=
          this.rowArray[index].exhaustPower * this.rowArray[index].exhaustNum;
      }
      this.$store.commit("sendTotalExhaustPower", totalexhaustPower);
      return totalexhaustPower;
    },
    exhaustInverterSum() {
      return this.rowArray.reduce(
        (accumulate, current) => accumulate + +current.exhaustInverter,
        0
      );
    },
    jetFanSum() {
      this.jetFanSumCalculate = this.rowArray.reduce(
        (accumulate, current) => accumulate + +current.jetFanNum,
        0
      );
      return this.jetFanSumCalculate;
    },
    jetFanPowerSum() {
      let totalJetFanPower = 0;
      for (let index = 0; index < this.rowArray.length; index++) {
        totalJetFanPower +=
          this.rowArray[index].jetFanPower * this.rowArray[index].jetFanNum;
      }
      this.$store.commit("sendTotalJetFanPower", totalJetFanPower);
      return totalJetFanPower;
    },
    getRowArrayFromStore() {
      this.totalRowArrayData = this.$store.state.Product.rowArray;
    },

    freshAirNumSumItems() {
      this.sumOfFreshAirNumItems = this.freshAirNumArray.reduce(
        (accumulate, current) => (accumulate += +current),
        0
      );
      return this.sumOfFreshAirNumItems;
    },
    ExhaustNumSumItems() {
      this.sumOfExhaustNumItems = this.ExhaustNumArray.reduce(
        (accumulate, current) => (accumulate += +current),
        0
      );
      return this.sumOfExhaustNumItems;
    },
    ThermalKeyFreshAirArrayReduce() {
      this.sumOfFreshAirThermalKey = this.thermalKeyFreshAirArray.reduce(
        (accumulate, current) => (accumulate += +current),
        0
      );
      return this.sumOfFreshAirThermalKey;
    },
    ThermalKeyExhaustArrayReduce() {
      this.sumOfExhaustThermalKey = this.thermalKeyExhaustArray.reduce(
        (accumulate, current) => (accumulate += +current),
        0
      );
      return this.sumOfExhaustThermalKey;
    },

    ContactorFreshAirReduce() {
      this.sumOfFreshAirContactor = this.contactorFreshAirArray.reduce(
        (accumulate, current) => (accumulate += +current),
        0
      );
    },
    ContactorExhaustReduce() {
      this.sumOfExhaustContactor = this.contactorExhaustArray.reduce(
        (accumulate, current) => (accumulate += +current),
        0
      );
    },
    ExhaustNumArrayDIReduce() {
      for (let length = 0; length < +this.zonesNumber; length++) {
        this.ExhaustNumArrayDI.length = 0;
        this.totalRowArrayData.map((element) => {
          if (element.exhaustInverter > 0) {
            this.ExhaustNumArrayDI.push(element.exhaustNum * 2);
          } else {
            this.ExhaustNumArrayDI.push(element.exhaustNum);
          }
        });
        this.sumOfExhaustNumDI = this.ExhaustNumArrayDI.reduce(
          (accumulate, current) => (accumulate += +current),
          0
        );
      }
    },
    FrshAirNumArrayDIReduce() {
      for (let length = 0; length < +this.zonesNumber; length++) {
        this.freshAirNumArrayDI.length = 0;
        this.totalRowArrayData.map((element) => {
          if (element.freshAirInverter > 0) {
            this.freshAirNumArrayDI.push(element.freshAirNum * 2);
          } else {
            this.freshAirNumArrayDI.push(element.freshAirNum);
          }
        });

        this.sumOfFreshAirNumArrayDI = this.freshAirNumArrayDI.reduce(
          (accumulate, current) => (accumulate += +current),
          0
        );
      }
    },
  },
  components: { BoardEquipmentTable },
  props: [
    "zonesNumber",
    "smokeCurtain",
    "DamperSupply",
    "DamperExhaust",
    "totalRelayNum",
    "hasHMI",
    "maxOfDamperSupplyDamperExhaust",
    "dICurtain",
    "dIAlarm",
    "floorsNumber",
  ],
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
.floorTable {
  display: none;
}
</style>
<template>
  <v-container>
    <v-card elevation="0">
      <v-card-title class="grey--text text-subtitle-2 ps-0">
        skeleton
      </v-card-title>
      <v-divider></v-divider>

      <table class="tableexhaust text-center ma-6">
        <tr>
          <td class="ps-2 pe-2">Width:</td>
          <td class="ps-2 pe-2">Height:</td>
          <td class="ps-2 pe-2">
            Montage Unit Cost: <br />
            Tooman Per CM
          </td>
          <td class="ps-2 pe-2">
            Skeleton Unit Cost: <br />
            Tooman Per CM
          </td>
        </tr>

        <tr>
          <td class="insertdata">
            <input
              type="number"
              value="150"
              id="Width"
              v-model="montageWidth"
            />
          </td>
          <td class="insertdata">
            <input
              type="number"
              value="200"
              id="Height"
              v-model="montageHeight"
            />
          </td>
          <td class="insertdata">
            <input
              type="number"
              value="500"
              id="montageUnitCost"
              v-model="montageUnitCost"
            />
          </td>
          <td class="insertdata">
            <input type="number" value="100" id="skeletonUnitCost" />
          </td>
        </tr>
      </table>
      <v-btn
        color="primary"
        class="ma-6"
        elevation="3"
        @click="CostCalculation()"
        >Submit</v-btn
      >
      <v-container v-if="showCostTable">
        <v-card-title class="grey--text text-subtitle-2 ps-0">
          Costs
        </v-card-title>
        <v-divider></v-divider>
        <table class="tableexhaust text-center ma-6">
          <tr>
            <td class="ps-2 pe-2">Programming Cost:</td>
            <td class="ps-2 pe-2">Montage Cost:</td>
            <td class="ps-2 pe-2">Skeleton Cost:</td>
          </tr>
          <tr>
            <td class="ps-2 pe-2">{{ ProgrammingCost }} Tooman</td>
            <td class="ps-2 pe-2">{{ montageCost }} Tooman</td>
            <td class="ps-2 pe-2">{{ skeletonCost }} Tooman</td>
          </tr>
        </table>
        {{ getDataFromStore }}
      </v-container>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      showCostTable: false,
      montageWidth: 150,
      montageHeight: 200,
      montageUnitCost: 500,
      montageCost: null,
      skeletonUnitCost: 100,
      totalDI: 0,
      totalDo: 0,
      DIProgrammingCost: 0,
      AIProgrammingCost: 0,
      ProgrammingCost: 0,
    };
  },

  methods: {
    CostCalculation() {
      this.showCostTable = true;
      this.montageCost =
        +this.montageUnitCost * +this.montageWidth * +this.montageHeight;
      this.skeletonCost =
        +this.skeletonUnitCost * +this.montageWidth * +this.montageHeight;
    },
  },
  computed: {
    getDataFromStore() {
      this.totalDI = this.$store.state.Product.totalDINum;
      this.totalAI = this.$store.state.Product.totalSensorNumber;
      this.DIProgrammingCost = this.$store.state.Product.DIProgrammingCost;
      this.AIProgrammingCost = this.$store.state.Product.AIProgrammingCost;
      this.ProgrammingCost =
        (this.totalDI * this.DIProgrammingCost +
          this.totalAI * this.AIProgrammingCost) *
        1000;
    },
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
</style>
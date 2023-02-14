<template>
  <v-card elevation="0">
    <table class="tableexhaust text-center ma-6">
      <tr>
        <td class="ps-2 pe-2">Sensor type (area covered by the circle):</td>
        <td class="ps-2 pe-2">
          The radius of the covered circle <br />
          (which is calculated with two decimal places):
        </td>
        <td class="ps-2 pe-2">
          Dimensions of the surrounding square <br />
          (square inside the circle):
        </td>
        <td class="ps-2 pe-2">
          Dimensions of the square enclosed by the inner circle:
        </td>
      </tr>
      <tr>
        <td class="insertdata">
          <input
            type="number"
            id="sensor"
            :disabled="!disableInputs"
            v-model="sensor"
          />
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="circleRadius"
            :disabled="!disableInputs"
            v-model="radius"
          />
          {{ circleRadius }}
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="squareDimension"
            :disabled="!disableInputs"
            v-model="squareDimension"
          />
          {{ squareDimensions }}
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="enclosedSquareDimension"
            :disabled="!disableInputs"
            v-model="enclosedSquareDimension"
          />
          {{ enclosedSquareDimensions }}
        </td>
      </tr>
      <tr>
        <td class="ps-2 pe-2">Average dimensions of two squares:</td>
        <td class="ps-2 pe-2">The number of sensors in a column:</td>
        <td class="ps-2 pe-2">The number of sensors in a row: <br /></td>
        <td class="ps-2 pe-2">Total number of sensors:</td>
      </tr>

      <tr>
        <td class="insertdata">
          <input
            type="number"
            id="averageDimensions"
            :disabled="!disableInputs"
            v-model="averageDimension"
          />
          {{ averageDimensions }}
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="columnSensorsNumber"
            :disabled="!disableInputs"
            v-model="columnSensorsNumber"
          />
          {{ columnSensorsNumbers }}
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="rowSensorsNumber"
            :disabled="!disableInputs"
            v-model="rowSensorsNumber"
          />
          {{ rowSensorsNumbers }}
        </td>
        <td class="insertdata">
          <input
            type="number"
            id="totalSensorsNumber"
            :disabled="!disableInputs"
            v-model="totalSensorNumber"
          />
        </td>
      </tr>
    </table>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      sensor: 200,
      radius: 0,
      squareDimension: null,
      enclosedSquareDimension: null,
      averageDimension: null,
      columnSensorsNumber: null,
      rowSensorsNumber: null,
      totalSensorNumber: null,
    };
  },
  methods: {
  //
  },

  computed: {
    squareDimensions() {
      this.squareDimension = (Math.sqrt(2) * this.radius).toFixed(2);
    },
    enclosedSquareDimensions() {
      this.enclosedSquareDimension = (this.radius * 2).toFixed(2);
    },
    averageDimensions() {
      this.averageDimension = (
        (+this.squareDimension + +this.enclosedSquareDimension) /
        2
      ).toFixed(2);
    },
    columnSensorsNumbers() {
      this.columnSensorsNumber = Math.ceil(
        +this.length / +this.averageDimension
      );
    },
    rowSensorsNumbers() {
      this.rowSensorsNumber = Math.ceil(+this.width / +this.averageDimension);
    },
    circleRadius() {
      if (this.disableInputs) {
        this.radius = Math.sqrt(this.sensor / Math.PI).toFixed(2);
      } else {
        null;
      }
    },
    totalSensorNumbers() {
      this.totalSensorNumber =
        +this.columnSensorsNumber + +this.rowSensorsNumber;
      this.$store.commit("sendTotalSensorNum", this.totalSensorNumber);
    },
  },
  props: ["disableInputs", "length", "width"],

  updated() {
    this.totalSensorNumbers;
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
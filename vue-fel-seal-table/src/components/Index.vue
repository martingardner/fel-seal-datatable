<template>
  <div>
    <h1>Human Classes</h1>
    <div v-if="filteredData">
      <table>
        <thead>
          <tr>
            <th v-for="n in theadNames" :key="n.name">
              {{n.name}}
              <span v-if="n.sort" @click="setSortCol(n.sort)">^</span>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(d, index) in filteredData" :key="index">
            <td>{{d.class}}</td>
            <td>{{d.hp}}</td>
            <td>{{d.mp}}</td>
            <td>{{d.atk}}</td>
            <td>{{d.def}}</td>
            <td>{{d.mnd}}</td>
            <td>{{d.res}}</td>
            <td>{{d.spd}}</td>
            <td>{{d.mv}}</td>
            <td>{{d.jump}}</td>
            <td>{{ getBoolText(d.swim) }}</td>
            <td>{{d.crit}}</td>
            <td>{{d.eva}}</td>
            <td>{{ getArrayFormattedData(d.wpn) }}</td>
            <td>{{ getBoolText(d.shield) }}</td>
            <td>{{ getArrayFormattedData(d.armor) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import data from "../data/data.js";

export default {
  name: "Index",
  data() {
    return {
      theadNames: [
        { name: "Class", sort: "class" },
        { name: "HP Growth", sort: "hp" },
        { name: "MP Growth", sort: "mp" },
        { name: "ATK Growth", sort: "atk" },
        { name: "DEF Growth", sort: "def" },
        { name: "MND Growth", sort: "mnd" },
        { name: "RES Growth", sort: "res" },
        { name: "SPD Growth", sort: "spd" },
        { name: "Move", sort: "mv" },
        { name: "Jump", sort: "jump" },
        { name: "Swim", sort: "swim" },
        { name: "Crit", sort: "crit" },
        { name: "Evasion", sort: "eva" },
        { name: "Weapon", sort: null },
        { name: "Shield", sort: "shield" },
        { name: "Armor", sort: null }
      ], //thead names
      tableData: data, //pure version of full data
      filteredData: data, //rendered subset of data
      sortCol: null, // for header sorting, which col by default
      sortASC: true // asc / desc
    };
  },
  methods: {
    getBoolText(bool) {
      return bool ? "Yes" : "No";
    },
    getArrayFormattedData(arr) {
      return arr.join(", ");
    },
    setSortCol(colname) {
      this.sortCol = colname;
      this.sortASC = !this.sortASC;
      this.sortFilteredData();
    },
    sortFilteredData() {
      this.filteredData.sort((a, b) => {
        if (this.sortASC) {
          return a[this.sortCol] > b[this.sortCol] ? 1 : -1;
        } else {
          return a[this.sortCol] > b[this.sortCol] ? -1 : 1;
        }
      });
    }
  }
};
</script>


<template>
  <div>
    <h1>Human Classes</h1>
    <div v-if="filterCheckboxes" class="filter-checkboxes">
      <h2>Class Filters</h2>
      <div v-for="f in filterCheckboxes" :key="f.param">
        <label>{{ f.name }}</label>
        <input type="checkbox" :name="f.param" @click="setFilterCheckboxesParam(f.param, $event)" />
      </div>
    </div>
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
        { name: "HP+", sort: "hp" },
        { name: "MP+", sort: "mp" },
        { name: "ATK+", sort: "atk" },
        { name: "DEF+", sort: "def" },
        { name: "MND+", sort: "mnd" },
        { name: "RES+", sort: "res" },
        { name: "SPD+", sort: "spd" },
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
      sortASC: true, // asc / desc
      filterCheckboxes: [], // created based off of data
      filterCheckboxesParam: [] //list of params checked in class filters
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
    setFilterCheckboxesParam(param, event) {
      if (event.target.checked && !this.filterCheckboxesContains(param)) {
        this.filterCheckboxesParam.push(param);
      } else {
        this.removeFromFilterCheckbox(param);
      }

      this.filterDataFromFilterCheckboxes();
    },
    filterCheckboxesContains(param) {
      return this.filterCheckboxesParam.includes(param);
    },
    removeFromFilterCheckbox(param) {
      const newArr = this.filterCheckboxesParam.filter(f => {
        if (param !== f) {
          return f;
        }
      });

      this.filterCheckboxesParam = newArr;
    },
    sortFilteredData() {
      this.filteredData.sort((a, b) => {
        if (this.sortASC) {
          return a[this.sortCol] > b[this.sortCol] ? 1 : -1;
        } else {
          return a[this.sortCol] > b[this.sortCol] ? -1 : 1;
        }
      });
    },
    filterDataFromFilterCheckboxes() {
      const newArr = this.tableData.filter(f => {
        if (this.filterCheckboxesParam.includes(f.param)) {
          return f;
        }
      });

      console.log("newArr", newArr);
      this.filterCheckboxesParam.length > 0
        ? (this.filteredData = newArr)
        : (this.filteredData = this.tableData);
    },
    returnFilterChecklistObj(data) {
      return {
        param: data.param,
        name: data.class,
        checked: false
      };
    },
    createFilterChecklist() {
      let tempArr = [];

      this.tableData.forEach(data => {
        tempArr.push(this.returnFilterChecklistObj(data));
      });

      this.filterCheckboxes = tempArr;
    }
  },
  created: function() {
    this.createFilterChecklist();
  }
};
</script>


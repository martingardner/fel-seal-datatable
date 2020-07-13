<template>
  <div>
    <h1>Human Classes</h1>
    <div v-if="filteredData">
      <table>
        <thead>
          <tr>
            <th>Class</th>
            <th>HP Growth</th>
            <th>MP Growth</th>
            <th>ATK Growth</th>
            <th>DEF Growth</th>
            <th>MND Growth</th>
            <th>RES Growth</th>
            <th>SPD Growth</th>
            <th>MV</th>
            <th>Jump</th>
            <th>Swim</th>
            <th>Crit</th>
            <th>Evasion</th>
            <th>Weapons</th>
            <th>Shields</th>
            <th>Armor</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(d, index) in tableData" :key="index">
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
      tableData: data, //pure version of full data
      filteredData: data //rendered subset of data
    };
  },
  methods: {
    getBoolText(bool) {
      return bool ? "Yes" : "No";
    },
    getArrayFormattedData(arr) {
      return arr.join(", ");
    }
  }
};
</script>


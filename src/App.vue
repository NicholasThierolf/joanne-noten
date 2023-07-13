<script setup>
import {reactive} from 'vue';

const lookup = [
  {
    from: 0,
    upto: 0.1,
    grade: "1"
  },
  {
    from: 0.2,
    upto: 1.7,
    grade: "1-"
  },
  {
    from: 1.8,
    upto: 2.8,
    grade: "2+"
  },
  {
    from: 2.9,
    upto: 3.9,
    grade: "2"
  },
  {
    from: 4,
    upto: 5.3,
    grade: "2-"
  },
  {
    from: 5.4,
    upto: 6.6,
    grade: "3+"
  },
  {
    from: 6.7,
    upto: 7.8,
    grade: "3"
  },
  {
    from: 7.9,
    upto: 9,
    grade: "3-"
  },
  {
    from: 9.1,
    upto: 10.4,
    grade: "4+"
  },
  {
    from: 10.5,
    upto: 11.7,
    grade: "4"
  },
  {
    from: 11.8,
    upto: 12.9,
    grade: "4-"
  },
  {
    from: 13,
    upto: 14,
    grade: "5+"
  },
  {
    from: 14.1,
    upto: 15.3,
    grade: "5"
  },
  {
    from: 15.4,
    upto: 16.6,
    grade: "5-"
  },
  {
    from: 16.7,
    upto: 17.9,
    grade: "6+"
  },
  {
    from: 18,
    upto: "∞",
    grade: "6"
  }
]

function calculateRows(){
  let lastMistakes = -0.5;
  return lookup.map(row => {
    let low = lastMistakes + 0.5;
    let high = Math.floor((row.upto*state.textLength/100)*2 + 0.09)/2;
    lastMistakes = high;
    if(row.upto == "∞" ) high = "∞";
    return {
      from: row.from,
      upto: row.upto,
      grade: row.grade,
      low: low,
      high: high
    }
  });
}

const state = reactive({
  textLength: 100,
  rows: []
})

state.rows = calculateRows();

</script>

<template>
  <main>
    <h1>Fehlerindexrechner</h1>
    <label>
      <span>Wörter:</span>
      <input type="number" v-model="state.textLength" @keyup="state.rows = calculateRows()" @change="state.rows = calculateRows()">
    </label>
    

    <table>
      <tr>
        <th>Note</th>
        <th>Mit {{ state.textLength }} Wörtern</th>
        <th>Index</th>
      </tr>
      <tr v-for="row in state.rows">
        <td>{{ row.grade }}</td>
        <td v-if="row.low == row.high">{{ row.low }}</td>
        <td v-else>{{ row.low }}F - {{ row.high }}F</td>
        <td>{{ `${row.from}`.replace(".", ",") }} - {{ `${row.upto}`.replace(".", ",") }}</td>
      </tr>
    </table>
  </main>
</template>

<style scoped lang="scss">
table {
  border: solid 1px var(--vt-c-text-dark-2);
  border-spacing: 0;
  border-collapse: collapse;
  td, th {
    border: solid 1px var(--vt-c-text-dark-2);
    padding: 0 20px;
    text-align: left;
  }
  th {
    font-weight: bold;
  }
}

h1 {
  font-weight: bold;
  font-size: 32px;
}

label {
  display: flex;
  flex-direction: column;
  margin-bottom: 30px;
  span {
    font-size: 22px;
  }
  input {
    background-color: transparent;
    color: var(--color-text);
    border: none;
    border-bottom: solid 1px gray;
    width: 100px;
    font-size: 16px;
  }
}

</style>

<style>
  #app {
    display: flex;
    justify-content: center;
  }
</style>
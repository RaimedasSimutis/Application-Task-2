<template>
  <div>
    <Table :data="testData" :columns="tableColumns"/>
  </div>
</template>

<script>
import axios from 'axios';
import Table from './Table.vue';

export default {
  name: 'HelloWorld',
  components: { Table },
  props: {
    msg: String,
  },
  data() {
    return {
      containersData: null,
      testData: [
        {
          id: 1,
        },
        {
          id: 2,
        },
        {
          id: 3,
        },
        {
          id: 4,
        },
        {
          id: 5,
        },
        {
          id: 6,
        },
        {
          id: 7,
        },
        {
          id: 8,
        },
        {
          id: 9,
        },
        {
          id: 10,
        },
        {
          id: 11,
        },
      ],
    };
  },
  computed: {
    tableColumns() {
      return [{

      }];
    },
  },
  mounted() {
    this.getData();
  },
  methods: {
    getData() {
      axios.get('https://raw.githubusercontent.com/vilnius/atlieku-tvarkymas/master/Data/konteineriai%20ir%20ju%20aptarnavimas/Konteineriu%20pakelimai.csv')
        .then((response) => {
          this.containersData = this.csvToArray(response.data);
        });
    },
    csvToArray(csv) {
      const lines = csv.split('\n');

      const result = [];
      const headers = lines[0].split(';');

      for (let i = 1; i < lines.length - 1; i++) {
        const obj = {};
        const currentLine = lines[i].split(';');

        for (let j = 0; j < headers.length; j++) {
          obj[headers[j]] = currentLine[j];
        }

        result.push(obj);
      }

      return result;
    },
  },
};
</script>

<style scoped lang="scss">
  th {
    border: 1px solid black;
  }

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

<template>
  <section class="content">
    <div class="table-container">
      <Table
        class="table"
        :data="testData"
        :columns="tableColumns"
        @cellClick="cellClicked"
      />
    </div>
    <Popup
      ref="ref-popup"
    />
  </section>

</template>

<script>
import axios from 'axios';
import Table from './Table.vue';
import Popup from './Popup.vue';

export default {
  name: 'HelloWorld',
  components: { Popup, Table },
  props: {
    msg: String,
  },
  data() {
    return {
      containersData: null,
      clickedCellData: null,
    };
  },
  computed: {
    tableColumns() {
      return [
        {
          title: 'ID',
          property: 'id',
        },
        {
          title: 'Title',
          property: 'title',
        },
        {
          title: 'Random',
          property: 'random',
        },
      ];
    },
    testData() {
      const newObj = [];
      for (let x = 0; x < 50; x++) {
        newObj.push({
          id: x + 1,
          title: `Name${Math.random() * 100}`,
          random: `Surname${Math.random() * 100}`,
        });
      }
      return newObj;
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
    cellClicked(cellData) {
      this.$refs['ref-popup'].openPopup(cellData);
      this.clickedCellData = cellData;
    },
  },
};
</script>

<style scoped lang="scss">
  .content {
    display: flex;
    justify-content: center;
    align-items: center;
  }

.table-container {
  /*height: 100vh;*/
  width: 70vw;
  padding: 50px;

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

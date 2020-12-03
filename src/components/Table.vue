<template>
  <table v-if="data">
    <tr>
      <th>No</th>
<!--      <th>ID</th>-->
<!--      <th>Auto</th>-->
<!--      <th>Pakelimo laikas</th>-->
<!--      <th>Duomenu patiekimo laikas</th>-->
    </tr>
    <tr v-for="(element, index) in  getData" :key="element['Pakelimo iraso ID']">
      <td>{{element.id}}</td>
      <td>{{index}}</td>
<!--      <td>{{element['Pakelimo iraso ID']}}</td>-->
<!--      <td>{{element.Automobilis}}</td>-->
<!--      <td>{{element['Pakelimo data, laikas']}}</td>-->
<!--      <td>{{element['Duomenu pateikimo data, laikas']}}</td>-->
    </tr>
    <tr>
      <div class="pagination">
      <button @click="previousPage">&laquo;</button>
<!--      <a href="#">1</a>-->
<!--      <a class="active" href="#">2</a>-->
<!--      <a href="#">3</a>-->
<!--      <a href="#">4</a>-->
<!--      <a href="#">5</a>-->
<!--      <a href="#">6</a>-->
      <button @click="nextPage">&raquo;</button>
        Current: {{currentPage}}
        Total: {{totalPages}}
    </div>
    </tr>
  </table>

</template>

<script>
export default {
  name: 'Table',
  props: {
    data: {
      required: true,
      type: Array,
    },
  },
  data() {
    return {
      currentPage: 0,
      firstPage: 0,
      lastPage: 0,
      visibleElements: 5,
      offset: 0,
    };
  },
  computed: {
    getData() {
      return this.data.filter((el, index) => {
        return index >= this.offset && index < this.offset + this.visibleElements;
      });
    },
    totalPages() {
      return Math.ceil(this.data.length / this.visibleElements);
    },
  },
  watch: {
    data() {
      console.log('data changed');
    },
  },
  methods: {
    nextPage() {
      if (this.currentPage > 0) {
        this.currentPage += 1;
      }
    },
    previousPage() {
      if (this.currentPage > 0) {
        this.currentPage -= 1;
      }
    },
    changePage(pageNumber) {
      console.log(pageNumber);
    },
  },
};
</script>

<style scoped>

</style>

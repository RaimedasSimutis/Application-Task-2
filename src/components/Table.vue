<template>
  <div>
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
    <select v-model="elementsPerPage">
      <option :value="5">5</option>
      <option :value="10">10</option>
      <option :value="15">15</option>
    </select>
  </div>
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
      elementsPerPage: 5,
      offset: 0,
    };
  },
  computed: {
    getData() {
      return this.data.filter((el, index) => {
        return index >= this.offset && index < this.offset + this.elementsPerPage;
      });
    },
    totalPages() {
      return Math.ceil(this.data.length / this.elementsPerPage);
    },
  },
  watch: {
    elementsPerPage() {
      this.currentPage = 0;
      this.offset = 0;
    },
    data() {
      console.log('data changed');
    },
  },
  methods: {
    nextPage() {
      if (this.currentPage < this.totalPages - 1) {
        this.currentPage += 1;
        this.offset += this.elementsPerPage;
      }
    },
    previousPage() {
      if (this.currentPage > 0) {
        this.currentPage -= 1;
        this.offset -= this.elementsPerPage;
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

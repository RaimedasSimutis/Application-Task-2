<template>
  <div>
    <table v-if="data">
    <tr>
      <th>ID</th>
      <th>Title</th>
<!--      <th>ID</th>-->
<!--      <th>Auto</th>-->
<!--      <th>Pakelimo laikas</th>-->
<!--      <th>Duomenu patiekimo laikas</th>-->
    </tr>
    <tr v-for="element in  getData" :key="element['Pakelimo iraso ID']">
      <td>{{element.id}}</td>
      <td>{{element.title}}</td>
<!--      <td>{{element['Pakelimo iraso ID']}}</td>-->
<!--      <td>{{element.Automobilis}}</td>-->
<!--      <td>{{element['Pakelimo data, laikas']}}</td>-->
<!--      <td>{{element['Duomenu pateikimo data, laikas']}}</td>-->
    </tr>
      <div class="pagination">
      <a class="pagination__button" @click="goToFirstPage">
        <img src="../assets/angle-double-left-solid.svg">
      </a>
      <a class="pagination__button" @click="goToPreviousPage">
        <img src="../assets/angle-left-solid.svg">
      </a>
      <a class="pagination__button" @click="goToNextPage">
        <img src="../assets/angle-right-solid.svg">
      </a>
      <a class="pagination__button" @click="goToLastPage">
        <img src="../assets/angle-double-right-solid.svg">
      </a>
        <p>Current: {{currentPage}}</p>
        <p>Total: {{totalPages}}</p>
    </div>
      <select v-model="elementsPerPage">
        <option :value="5">5</option>
        <option :value="10">10</option>
        <option :value="15">15</option>
        <option value="all">All</option>
      </select>
  </table>

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
      elementsPerPage: 10,
      offset: 0,
    };
  },
  computed: {
    getData() {
      if (this.elementsPerPage === 'all') {
        return this.data;
      }
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
      this.goToFirstPage();
    },
    data() {
      console.log('data changed');
    },
  },
  methods: {
    goToNextPage() {
      if (this.currentPage < this.totalPages - 1) {
        this.currentPage += 1;
        this.offset += this.elementsPerPage;
      }
    },
    goToPreviousPage() {
      if (this.currentPage > 0) {
        this.currentPage -= 1;
        this.offset -= this.elementsPerPage;
      }
    },
    goToFirstPage() {
      this.currentPage = 0;
      this.offset = 0;
    },
    goToLastPage() {
      this.currentPage = this.totalPages;
      this.offset = this.elementsPerPage * (this.totalPages - 1);
    },
    changePage(pageNumber) {
      console.log(pageNumber);
    },
  },
};
</script>

<style scoped lang="scss">
.pagination {
  &__button {

    img {
      padding: 0 10px;
      height: 20px;

    }

    &:hover {
      cursor: pointer;
      /*background: rgba(135,135,135,0.43);*/
    }
  }
}
</style>

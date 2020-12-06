<template>
    <table class="table" v-if="data">
      <thead class="table__head">
        <tr class="table__row table__row--head">
          <th
            v-for="(column, columnsIndex) in columns"
            :key="columnsIndex"
            @click="changeSort(column.property)"
            class="table__cell table__cell--head"
            :class="{'sort-active' : sortByProperty === column.property}"
          >
            {{column.title}}
            <span v-if="sortByProperty === column.property">
              <img
                class="sort-icon"
                v-if="sortByType==='asc'"
                src="../assets/caret-up-solid.svg"/>
              <img class="sort-icon"
                 v-if="sortByType==='desc'"
                 src="../assets/caret-down-solid.svg"/>
            </span>
          </th>
      </tr>
      </thead>

      <tbody class="table__body">
        <tr class="table__row" v-for="(element, elementIndex) in getData" :key="elementIndex">
          <td
            class="table__cell" v-for="(column, columnIndex) in columns"
            :key="columnIndex"
            @click="emitLastClickedCellData(element[column.property])"
          >
            {{element[column.property]}}
          </td>
        </tr>
      </tbody>

      <tfoot class="table__foot">
        <tr class="table__row table__pagination" >
          <td class="table__cell" align="left" colspan="1">
              <select class="table__pagination-select" v-model="elementsPerPage">
                <option :value="5">5</option>
                <option :value="10">10</option>
                <option :value="15">15</option>
                <option value="all">All</option>
              </select>
          </td>

          <td class="table__cell" align="right" colspan="2">
            <span class="">
               <a class="table__pagination-button" @click="goToFirstPage">
                <img src="../assets/angle-double-left-solid.svg">
              </a>
              <a class="table__pagination-button" @click="goToPreviousPage">
                <img src="../assets/angle-left-solid.svg">
              </a>
              <a class="table__pagination-button" @click="goToNextPage">
                <img src="../assets/angle-right-solid.svg">
              </a>
              <a class="table__pagination-button" @click="goToLastPage">
                <img src="../assets/angle-double-right-solid.svg">
              </a>
            </span>

          </td>
        </tr>
      </tfoot>
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
    columns: {
      required: true,
    },
  },
  data() {
    return {
      currentPage: 0,
      firstPage: 0,
      lastPage: 0,
      elementsPerPage: 10,
      offset: 0,
      sortByProperty: 'title',
      sortByType: 'desc',
    };
  },
  computed: {
    getData() {
      this.sortBy(this.sortByProperty, this.sortByType);

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
    sortBy(sortProperty, sortType) {
      this.data.sort((a, b) => {
        // console.log(a[sortProperty], b[sortProperty]);
        const nameA = a[sortProperty];
        const nameB = b[sortProperty];
        if (sortType === 'asc') {
          if (nameA < nameB) {
            return -1;
          }
          if (nameA > nameB) {
            return 1;
          }
        } else if (sortType === 'desc') {
          if (nameA < nameB) {
            return 1;
          }
          if (nameA > nameB) {
            return -1;
          }
        }

        return 0;
      });
    },
    changeSort(sortProperty) {
      if (this.sortByProperty === sortProperty) {
        this.sortByType = this.sortByType === 'asc' ? 'desc' : 'asc';
      } else {
        this.sortByProperty = sortProperty;
      }
    },
    emitLastClickedCellData(cellData) {
      this.$emit('cellClick', cellData);
    },
  },
};
</script>

<style scoped lang="scss">
  .sort-active {
    /*background-color: #42b983;*/
  }

  .sort-icon {
    height: 17px;
  }

  .table {
    height: 100%;
    width: 100%;
    background-color: #ffffff;
    border-radius: 10px;
    border-collapse: collapse;
    overflow: hidden;

    &__head {
      background-color: #6c7ae0;
      color: #ffffff;
    }

    &__body {

    }

    &__foot{

    }

    &__cell {
      padding: 20px;
      font-size: 15px;

      &--head {
        font-size: 18px;
      }
    }

    &__row {
      border-bottom: 1px solid #f2f2f2;
      height: 60px;
    }

    &__pagination {
      /*background-color: red;*/
      /*height: 60px;*/

    }

    &__pagination-select {
      /*margin-left: 40px;*/
    }

    &__pagination-button {
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

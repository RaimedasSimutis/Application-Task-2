<template>
    <table class="table" v-if="dataArr">
      <thead class="table__head">
        <tr class="table__row table__row--head">
          <th
            v-for="(column, columnsIndex) in columns"
            :key="columnsIndex"
            class="table__cell table__cell--head"
            :class="{'table__cell--sort-active' : sortByProperty === column.property}"
            :align="column.align"
            @click="changeSort(column.property)"
          >
            {{column.title}}
            <span v-if="sortByProperty === column.property">
              <img
                class="table__sort-icon"
                v-if="sortByType==='asc'"
                src="../assets/caret-up-solid.svg"
                alt="asc"
              />
              <img class="table__sort-icon"
                 v-if="sortByType==='desc'"
                 src="../assets/caret-down-solid.svg"
                 alt="desc"
              />
            </span>
          </th>
        </tr>
      </thead>

      <tbody class="table__body">
        <tr
          class="table__row table__row--body"
          v-for="(element, elementIndex) in getData"
          :key="elementIndex"
          :class="{'table__row--highlighted': highlightedRowIndex === elementIndex}"
          @click="highlightRow(elementIndex)"
        >
          <td
            class="table__cell" v-for="(column, columnIndex) in columns"
            :key="columnIndex"
            :align="column.align"
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
              <option :value="20">20</option>
              <option :value="50">50</option>
              <option value="all">All</option>
            </select>
          </td>

          <td class="table__cell" align="right" :colspan="columns.length - 1">
            <span>
              <a class="table__pagination-button" @click="goToFirstPage">
                <img src="../assets/angle-double-left-solid.svg" alt="angle-dbl-left">
              </a>
              <a class="table__pagination-button" @click="goToPreviousPage">
                <img src="../assets/angle-left-solid.svg" alt="angle-left">
              </a>
              <a class="table__pagination-button" @click="goToNextPage">
                <img src="../assets/angle-right-solid.svg" alt="angle-right">
              </a>
              <a class="table__pagination-button" @click="goToLastPage">
                <img src="../assets/angle-double-right-solid.svg" alt="angle-dbl-right">
              </a>
            </span>
            <span class="table__pagination-status">{{currentPage+1}}/{{totalPages}}</span>
          </td>
        </tr>
      </tfoot>
    </table>
</template>

<script>
export default {
  name: 'Table',
  props: {
    dataArr: {
      required: true,
    },
    columns: {
      required: true,
    },
  },
  data() {
    return {
      currentPage: 0,
      elementsPerPage: 10,
      offset: 0,
      sortByProperty: null,
      sortByType: null,
      highlightedRowIndex: null,
    };
  },
  computed: {
    getData() {
      const sortedData = this.sortBy(this.sortByProperty, this.sortByType);

      if (this.elementsPerPage === 'all') {
        return sortedData;
      }

      return sortedData.filter((el, index) => {
        return index >= this.offset && index < this.offset + this.elementsPerPage;
      });
    },
    totalPages() {
      return Math.ceil(this.dataArr.length / this.elementsPerPage) || 1;
    },
  },
  watch: {
    elementsPerPage() {
      this.goToFirstPage();
    },
  },
  methods: {
    goToNextPage() {
      if (this.currentPage < this.totalPages - 1) {
        this.currentPage += 1;
        this.offset += this.elementsPerPage;
        this.highlightedRowIndex = null;
      }
    },
    goToPreviousPage() {
      if (this.currentPage > 0) {
        this.currentPage -= 1;
        this.offset -= this.elementsPerPage;
        this.highlightedRowIndex = null;
      }
    },
    goToFirstPage() {
      this.currentPage = 0;
      this.offset = 0;
      this.highlightedRowIndex = null;
    },
    goToLastPage() {
      this.currentPage = this.totalPages - 1;
      this.offset = this.elementsPerPage * (this.totalPages - 1);
      this.highlightedRowIndex = null;
    },
    sortBy(sortProperty, sortType) {
      const dataArrClone = JSON.parse(JSON.stringify(this.dataArr));
      return dataArrClone.sort((a, b) => {
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
      this.goToFirstPage();

      if (this.sortByProperty === sortProperty) {
        this.sortByType = this.sortByType === 'asc' ? 'desc' : 'asc';
      } else {
        this.sortByProperty = sortProperty;
        this.sortByType = 'asc';
      }
    },
    emitLastClickedCellData(cellData) {
      this.$emit('cellClick', cellData);
    },
    highlightRow(rowIndex) {
      this.highlightedRowIndex = rowIndex;
    },
  },
};
</script>

<style scoped lang="scss">
  .table {
    height: 100%;
    width: 100%;
    background-color: #ffffff;
    border-radius: 10px;
    border-collapse: collapse;
    overflow: hidden;
    box-shadow: 0 0 40px 0 rgba(0,0,0,.15);
    -moz-box-shadow: 0 0 40px 0 rgba(0,0,0,.15);

    &__head {
      background-color: #6c7ae0;
      color: #ffffff;
    }

    &__sort-icon {
      height: 17px;
      margin-left: 5px;
      filter:
        invert(100%)
        sepia(100%)
        saturate(0%)
        hue-rotate(288deg)
        brightness(102%)
        contrast(102%);
    }

    &__cell {
      padding: 20px;
      font-size: 15px;

      &--head {
        font-size: 18px;
      }

      &--sort-active {
        background-color: rgba(0, 0, 0, 0.11);
      }
    }

    &__row {
      border-bottom: 1px solid #f2f2f2;
      height: 60px;

      &--highlighted {
        background-color: #ececff;
      }

      &--head {
        cursor: pointer;
      }

      &--body {
        cursor: pointer;

        &:hover {
          background-color: #ececff;
        }
      }
    }

    &__pagination-status {
      font-weight: bold;
    }

    &__pagination-button {
      img {
        padding: 0 10px;
        height: 20px;
        position: relative;
        top: 4px;
      }

      &:hover {
        cursor: pointer;
      }
    }

  }
</style>

<template>
  <div>
    <div class="main__search">
      <label for="search-iniput">Search:</label>
      <input v-model="searchData" id="search-iniput" type="text" />
    </div>
    <table class="main__custom-table">
      <thead>
        <tr>
          <th v-for="(column, columnName) in columns" :key="columnName">
            <SortButtons v-if="column.sort" v-model="sort" :column="columnName">
              <template #headerName>
                <span>{{ column.name }}</span>
              </template>
            </SortButtons>
            <span v-else>{{ column.name }}</span>
          </th>
        </tr>
      </thead>

      <tbody>
        <template v-if="tableRows.length > 0">
            <tr v-for="(row, i) in tableRows" :key="i">
                <td v-for="(column, columnName) in columns" :key="columnName">
                    <template v-if="$slots[columnName]">
                    <slot :name="columnName" :row="row" :field="row[columnName]" />
                    </template>
                    <span v-else>{{ row[columnName] }}</span>
                </td>
            </tr>
        </template>
        <tr v-else>
            <td colspan="100%">No data to be shown.</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import SortButtons from "@/components/SortButtons.vue";

export default {
  components: {
    SortButtons,
  },
  props: {
    columns: {
      type: Object,
      required: true,
    },
    rows: {
      type: Array,
      default: [],
    },
  },
  data() {
    return {
      searchData: "",
      sort: {
        by: "",
        direction: "",
      },
    };
  },
  computed: {
    tableRows() {
      const rows = this.filterRows(this.searchData);

      return this.sortRows(rows);
    },
  },
  methods: {
    filterRows(target) {
      if (target !== "") {
        return [...this.rows].filter((row) => {
          for (const field in row) {
            if (this.fieldIncludesTarget(row[field], target)) {
              return true;
            }
          }
        });
      }

      return this.rows;
    },

    fieldIncludesTarget(field, target) {
      const fieldAsString = field.toString().toLowerCase();

      return fieldAsString.includes(target.toLowerCase());
    },

    sortRows(rows) {
      if (this.sort.by !== "") {
        
        rows.sort((a, b) => {
          const fa = this.dataTypeSort(a[this.sort.by], this.columns[this.sort.by].sort || "");
          const fb = this.dataTypeSort(b[this.sort.by],this.columns[this.sort.by].sort || "");

          if (this.sort.direction === "ascending") {
            return this.sortAscending(fa, fb);
          }

          return this.sortDescending(fa, fb);
        });
      }

      return rows
    },

    dataTypeSort(value, key) {
      switch (key) {
        case "number":
          return parseInt(value);
        default:
          if (value) {
            return value.toString().toLowerCase();
          }

          return "";
      }
    },

    sortAscending(a, b) {
      if (a > b) {
        return -1;
      }
      if (a < b) {
        return 1;
      }
      return 0;
    },

    sortDescending(a, b) {
      if (a < b) {
        return -1;
      }
      if (a > b) {
        return 1;
      }
      return 0;
    },
  },
};
</script>

<style lang="scss" scoped>
.main__search {
    text-align: end;
    margin-bottom: 1rem;

    label {
        color: var(--my-white);
        margin-right: 0.5rem;
    }
}

.main__custom-table {
    border-spacing: 0px;
    border-radius: 10px;
    border-collapse: separate;
    border: solid transparent 1px;
    overflow: hidden;
    width: 100%;
    table-layout: fixed;
    max-width: 1000px;

    @media screen and (max-width: 650px) {
        display: block;
        overflow: auto;
    }
  
  thead {
    color: var(--my-white);
    background-color: var(--my-dark-bg);
  }

  tbody {
    color: var(--my-black);

    tr:nth-child(odd) {
      background-color: white;
    }

    tr:nth-child(even) {
      background-color: rgb(243 243 243);
    }
  }

  th,
  td {
    padding: 10px;
    text-align: center;
    width: 100%;
  }

  @media screen and (max-width: 1120px) {
    td {
        overflow: hidden; 
        text-overflow: ellipsis; 
        word-wrap: break-word;
    }
  }
}
</style>

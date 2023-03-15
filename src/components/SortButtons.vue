<template>
  <div 
    class="main__sort-button" 
    @click="applyOrder()"
    @keyup.enter="applyOrder()"
    role="button"
    aria-pressed="false"
    tabindex="0"
   >
    <slot name="headerName" />
    <div class="wrap__icons">
      <img
        class="arrow-up"
        :class="{ appear: appearAscending, 'no-sort': noSortApplied }"
        src="arrow-up.svg"
        alt="Arrow up"
      />

      <img
        :class="{ appear: appearDescending, 'no-sort': noSortApplied }"
        src="arrow-down.svg"
        alt="Arrow down"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    modelValue: {
      type: Object,
      required: true,
    },
    column: {
      type: String,
      required: true,
    },
  },
  emits: ["update:modelValue"],
  data() {
    return {
      sort: this.modelValue,
    };
  },
  computed: {
    appearAscending() {
      return (
        this.sort.direction === "" ||
        this.sort.direction === "ascending" ||
        this.sort.by !== this.column
      );
    },
    appearDescending() {
      return (
        this.sort.direction === "" ||
        this.sort.direction === "descending" ||
        this.sort.by !== this.column
      );
    },
    noSortApplied() {
      return this.appearAscending && this.appearDescending;
    },
    noSortApplied() {
      return this.sort.by === "" || this.sort.by !== this.column;
    },
    isAscendingDirection() {
      return (
        this.sort.direction === "ascending" && this.sort.by === this.column
      );
    },
  },
  methods: {
    applyOrder() {
      if (this.isAscendingDirection) {
        this.sort.direction = 'descending'
      } else {
        this.sort.direction = 'ascending'
      }
      
      this.sort.by = this.column
      this.$emit("update:modelValue", this.sort);
    },
  },
};
</script>

<style scoped lang="scss">
.main__sort-button {
    display: flex;
    align-items: center;
    gap: 5px;
    width: 100%;
    height: 100%;
    justify-content: center;
    cursor: pointer;

    &:focus, &:visited {
        border: 1px solid rgb(93, 93, 93);
    }

    .wrap__icons {
    display: grid;
    grid-template-columns: 1fr;
    }

    img {
    display: none;
    width: 15px;
    }

    .no-sort {
    opacity: 0.6;
    }
    .appear {
    display: block;
    margin-bottom: 2px;
    }
}
</style>

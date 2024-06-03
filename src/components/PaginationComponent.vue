
<script>
export default {
  name: 'PaginationComponent',
  props: {
    currentPage: Number,
    hasNextPage: Boolean
  },
  methods: {
    nextPage() {
      if (this.hasNextPage) {
        this.$emit('page-change', this.currentPage + 1);
      }
    },
    prevPage() {
      this.$emit('page-change', this.currentPage - 1);
    }
  }
}
</script>
<template>
  <div class="pagination">
    <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
    <button @click="nextPage" :disabled="!hasNextPage">Next</button>
  </div>
</template>


<style>
.pagination {
  margin-top: 16px;
}
</style>

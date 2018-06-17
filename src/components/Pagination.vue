<template>
    <div class="pagination__container">
        <a @click="prevPage" class="pagination"><</a>
        <a @click="updatePage(1)"
            class="pagination"
            :class="{
                'pagination-isActive': isActive(1)
            }">{{ 1 }}</a>
        <a v-if="currentPage > 5" class="pagination pagination-dots">...</a>
        <a v-for="page in pageSum"
            :key="page"
            v-if="page != 1 && page != pageSum && page >= currentPage - 2 && page <= currentPage + 2"
            @click="updatePage(page)"
            class="pagination"
            :class="{
                'pagination-isActive': isActive(page)
            }">{{ page }}</a>
        <a v-if="currentPage < pageSum - 3" class="pagination pagination-dots">...</a>
        <a @click="updatePage(pageSum)"
            class="pagination"
            :class="{
                'pagination-isActive': isActive(pageSum)
            }">{{ pageSum }}</a>
        <a @click="nextPage" class="pagination">></a>
    </div>
</template>

<script>
export default {
    props: {
        pageSum: {type: Number},
        currentPage: {type: Number}
    },
    data() {
        return {

        }
    },
    methods: {
        updatePage(val) {
            this.$emit('update-page', val);
        },
        prevPage() {
            if(this.currentPage - 1 >= 1) {
                this.$emit('update-page', this.currentPage - 1);
            }
        },
        nextPage() {
            if(this.currentPage + 1 <= this.pageSum) {
                this.$emit('update-page', this.currentPage + 1);
            }
        },
        isActive(p) {
            return this.currentPage == p;
        }
    }
}
</script>

<style lang="scss">
$main-purple: #9013fe;
.pagination__container {
  display: flex;
  justify-content: flex-end;
}
.pagination {
  flex: 0 0 15px;
  cursor: pointer;
  color: $main-purple;
  background: white;
  padding: 15px;
  border: 1px solid #eee;
  border-right: none;
  &-isActive {
    color: white;
    background: $main-purple;
  }
  &-dots {
    border-color: transparent;
    background: transparent;
    cursor: default;
  }
}
</style>
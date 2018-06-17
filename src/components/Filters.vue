<template>
    <div class="filters__container">
        <div class="filter">
            <p class="filter__title">地區</p>
            <select class="regionSelect" v-model="selectedRegion">
                <option value="">請選擇地區</option>
                <option v-for="r in allRegion" :value="r">{{ r }}</option>
            </select>
        </div>
        <div class="filter">
            <p class="filter__title">進階條件</p>
            <div v-for="f in filters" class="filter__item"><label class="filter__label"><input type="checkbox" v-model="selectedFilters" :value="f">{{ f }}</label></div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        filters: {type: Array},
        allRegion: {type: Array}
    },
    watch: {
        selectedRegion(val) {
            this.$emit('select-region', val);
        },
        selectedFilters(val) {
            this.$emit('select-filters', val);
        }
    },
    data() {
        return {
            selectedRegion: '',
            selectedFilters: []
        }
    }
}
</script>
<style lang="scss">
.filters__container {
  flex-basis: 300px;
}
.filter {
  border-bottom: 1px solid #d7d7d7;
  padding: 20px;
  background-color: #ebebeb;
}
.filter__title {
  text-align: left;
  font-weight: bold;
}
.regionSelect {
  height: 34px;
  width: 100%;
  border-color: #eee;
}
.filter__item {
  margin-bottom: 10px;
  text-align: left;
}
.filter__label {
  cursor: pointer;
  > input {
    margin-right: 10px;
  }
}
</style>

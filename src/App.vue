<template>
  <div id="app">
    <app-header 
		ref="appHeader"
		@update-keywords="updateKeywords">
	</app-header>
	<div class="body container">
		<filters 
			ref="filters"
			:all-region="allRegion"
			:filters="filters"
			@select-region="updateRegion"
			@select-filters="updateFilters">
		</filters>
		<div class="article__container">
			<template v-if="!isViewingArticle">
				<div class="resultSection">
					<p class="resultSection__title">共有 {{ filteredActivities.length }} 個搜尋結果</p>
					<div @click="clearSelectedRegion" v-if="selectedRegion.length > 0" class="filterTag">{{ selectedRegion }} x</div>
					<div @click="clearKeywords" v-if="keywords.trim().length > 0" class="filterTag">{{ keywords }} x</div>
					<div v-for="f in selectedFilters" @click="clearSelectedFilter(f)" v-if="selectedFilters.length > 0" class="filterTag">{{ f }} x</div>
				</div>
				<activity-item 
					v-for="i in pageActivities" 
					:activity="i" :key="i.Id"
					@click-item="clickItem(i)"></activity-item>
				<pagination 
					:current-page="currentPage"
					:page-sum="pageSum" 
					@update-page="updatePage">
				</pagination>
			</template>
			<template v-else>
				<button class="btn-default" @click="backToLast">上一頁</button>
				<app-article :selected-article="selectedArticle"></app-article>
			</template>
		</div>
	</div>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Filters from './components/Filters.vue';
import ActivityItem from './components/ActivityItem.vue';
import Article from './components/Article.vue';
import Pagination from './components/Pagination.vue';

export default {
	name: 'App',
	data() {
		return {
			activityItems: [],
			isViewingArticle: false,
			allRegion: [],

			pageSum: 0,
			currentPage: 1,
			itemPerPage: 10,

			selectedRegion: '',
			selectedFilters: [],
			filters: ['免費參觀', '全天候開放'],
			keywords: '',
			selectedArticle: {}
		}
	},
	created() {
		this.fetchApiData();
	},
	computed: {
		filteredActivities() {
			var regionActivities = this.selectedRegion == '' ? this.activityItems : this.activityItems.filter(ele => {
				return ele.Zone == this.selectedRegion;
			});
			var filteredActivities = this.selectedFilters.includes('全天候開放') ?
				regionActivities.filter(ele => {
					return ele.Opentime == '全天候開放'
				}) : regionActivities;
			filteredActivities = this.selectedFilters.includes('免費參觀') ?
				filteredActivities.filter(ele => {
					return ele.Ticketinfo == '免費參觀'
				}) : filteredActivities;

			filteredActivities = filteredActivities.filter(ele => {
				return ele.Name.includes(this.keywords.trim()) || ele.Description.includes(this.keywords.trim());
			});
			return filteredActivities;
		},
		pageActivities() {
			return this.filteredActivities.slice((this.currentPage - 1) * this.itemPerPage, this.currentPage * this.itemPerPage);
		}
	},
	watch: {
		filteredActivities(val) {
			this.pageSum = Math.ceil(val.length / this.itemPerPage);
			this.currentPage = 1;
		}
	},
	methods: {
		fetchApiData() {
			const url = 'https://data.kcg.gov.tw/api/action/datastore_search?resource_id=92290ee5-6e61-456f-80c0-249eae2fcc97';
			this.$http.get(`${url}&limit=300`).then(res => {
				this.activityItems = res.body.result.records;
				this.pageSum = Math.ceil(res.body.result.total / this.itemPerPage);
				this.createdAllRegion();
			})
		},
		createdAllRegion() {
			var arr = [];
			for(let k in this.activityItems) {
				let thisZone = this.activityItems[k].Zone;
				if(!arr.includes(thisZone)) {
					arr.push(thisZone);
				}
			}
			this.allRegion = arr;
		},
		clickItem(index) {
			console.log(index);
			this.isViewingArticle = true;
			this.selectedArticle = index;
		},
		backToLast() {
			this.isViewingArticle = false;
		},
		updatePage(val) {
			this.currentPage = val;
		},
		updateRegion(val) {
			this.selectedRegion = val;
		},
		updateFilters(val) {
			this.selectedFilters = val;
		},
		updateKeywords(val) {
			this.keywords = val;
		},
		clearSelectedRegion() {
			this.$refs.filters.selectedRegion = '';
		},
		clearKeywords() {
			this.$refs.appHeader.keywords = '';
		},
		clearSelectedFilter(target) {
			var index = this.$refs.filters.selectedFilters.indexOf(target);
			this.$refs.filters.selectedFilters.splice(index, 1);
		}
	},
	components: {
		'app-header': Header,
		Filters,
		ActivityItem,
		'app-article': Article,
		Pagination
	}
}
</script>

<style lang="scss">
$main-purple: #9013fe;
#app {
  font-family: "Roboto Regular", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: #f2f2f2;
}
.w100 {
  width: 100%;
}
.title {
  font-family: Roboto-Medium;
  font-size: 24px;
  color: #000000;
}
.title2 {
  font-family: Roboto-Medium;
  font-size: 36px;
  color: #000000;
}
.subTitle {
  font-family: Roboto-Medium;
  font-size: 20px;
  color: #000000;
}
.remark {
  font-family: Roboto-Regular;
  font-size: 16px;
  color: #000000;
  line-height: 24px;
}
.body {
  display: flex;
  @media screen and (max-width: 769px) {
    flex-direction: column;
  }
}
.container {
  max-width: 1200px;
  margin: 0 auto;
}
.article__container {
  flex: 1;
  padding: 30px 50px;
  @media screen and (max-width: 769px) {
    padding: 10px;
  }
}
.mT10 {
  margin-top: 10px;
}
.btn-default {
  padding: 6px 20px;
  background: white;
  color: $main-purple;
  border-radius: 5px;
  border-color: transparent;
  cursor: pointer;
}
.resultSection {
  margin-bottom: 20px;
}
.resultSection__title {
  font-size: 18px;
  color: #9c9c9c;
}
.filterTag {
  border: 1px solid $main-purple;
  color: $main-purple;
  font-size: 16px;
  display: inline-block;
  border-radius: 50px;
  padding: 10px;
  margin-right: 5px;
  cursor: pointer;
  transition: all 200ms ease-out;
  &:hover {
    color: white;
    background: $main-purple;
  }
}
</style>

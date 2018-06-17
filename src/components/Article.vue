<template>
    <div class="article">
        <div class="article__imgContainer">
            <img class="article__img" :src="selectedArticle.Picture1" alt="">
        </div>
        <div class="article__content">
            <p class="article__title">{{ selectedArticle.Name }}</p>
            <span v-if="selectedArticle.Ticketinfo != ''" class="article__tag">{{ selectedArticle.Ticketinfo }}</span>
            <div class="article__icons">
                <div>
                    <i class="fas fa-map-marker-alt"></i><span class="article__remark">{{ selectedArticle.Add }}</span>
                </div>
                <div class="mT10">
                    <i class="far fa-calendar-alt"></i><span class="article__remark">{{ selectedArticle.Opentime }}</span>
                </div>
            </div>
            <p class="article__desc">{{ selectedArticle.Toldescribe }}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        selectedArticle: {type: Object}
    },
    data() {
        return {

        }
    }
}
</script>
<style lang="scss">
$main-purple: #9013fe;
.article {
  margin-top: 15px;
}
.article__imgContainer {
  height: 400px;
}
.article__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.article__content {
  padding: 30px 50px;
  background: white;
}
.article__title {
  font-size: 36px;
  color: $main-purple;
  margin: 0 0 15px;
}
.article__desc {
  font-size: 16px;
  color: rgb(58, 58, 58);
  line-height: 28px;
  margin-top: 20px;
}
.article__tag {
  display: inline-block;
  padding: 5px 20px;
  font-size: 14px;
  background-color: #d7d7d7;
  color: white;
  border-radius: 50px;
}
.article__icons {
  color: #9c9c9c;
  margin-top: 20px;
}
.article__remark {
  margin-right: 15px;
  margin-left: 7px;
  font-size: 14px;
}
</style>
<template>
    <div @click="clickItem" class="activityItem">
        <div class="activityItem__media">
            <img class="" :src="activity.Picture1" alt="">
        </div>
        <div class="activityItem__content">
            <p class="activityItem__name title">{{ activity.Name }}</p>
            <p class="activityItem__desc">{{ activity.Description.slice(0, 80) }}...</p>
            <span v-if="activity.Ticketinfo != ''" class="activityItem__tag">{{ activity.Ticketinfo }}</span>
            <div class="activityItem__bottom">
                <i class="fas fa-map-marker-alt"></i><span class="activityItem__remark">{{ activity.Zone }}</span>
                <i class="far fa-calendar-alt"></i><span class="activityItem__remark">{{ activity.Opentime }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        activity: {type: Object}
    },
    data() {
        return {

        }
    },
    methods: {
        clickItem() {
            this.$emit('click-item');
        }
    }
}
</script>
<style lang="scss">
$main-purple: #9013fe;
.activityItem {
  cursor: pointer;
  display: flex;
  margin-bottom: 30px;
  box-shadow: 0px 5px 30px -5px #ebd4ff;
  transition: all 200ms ease-out;
  border-radius: 5px;
  overflow: hidden;
  &:hover {
    box-shadow: 0px 0px 15px 0px #a53dff;
  }
  @media screen and (max-width: 900px) {
    flex-direction: column;
  }
}
.activityItem__media {
  flex-basis: 250px;
  background: white;
  > img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}
.activityItem__content {
  flex: 1;
  padding: 40px 30px;
  background-color: white;
}
.activityItem__name {
  color: $main-purple;
  margin: 0;
}
.activityItem__desc {
  font-size: 16px;
  line-height: 24px;
  color: rgb(58, 58, 58);
}
.activityItem__tag {
  display: inline-block;
  padding: 5px 20px;
  font-size: 14px;
  background-color: #d7d7d7;
  color: white;
  border-radius: 50px;
}
.activityItem__remark {
  font-size: 14px;
  margin: 0 15px 0 7px;
}
.activityItem__bottom {
  margin-top: 10px;
  color: #9c9c9c;
}
</style>
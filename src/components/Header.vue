<template>
    <div class="header">
        <div class="header__logo">
            <img @click="clickLogo" src="../assets/logo.png" alt="">
        </div>
        <div class="header__search">
            <input class="keywordInput header__keywordInput" v-model="keywords" type="text" placeholder="輸入關鍵字">
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      keywords: ''
    }
  },
  watch: {
    keywords(val) {
      this.$emit('update-keywords', val);
    }
  },
  methods: {
    clickLogo() {
      this.$emit('click-logo');
    }
  }
}
</script>

<style lang="scss">
$main-purple: #9013fe;
.header {
  background-color: $main-purple;
  height: 70px;
  display: flex;
  @media screen and (max-width: 769px) {
    justify-content: center;
    flex-direction: column;
    height: auto;
  }
}
.header__logo {
  flex: 0 0 250px;
  display: flex;
  justify-content: center;
  align-items: center;
  > img {
    width: 130px;
    cursor: pointer;
  }
  @media screen and (max-width: 769px) {
    flex: 50px;
  }
}
.header__search {
  padding-right: 50px;
  flex: 1;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  @media screen and (max-width: 769px) {
    padding-right: 0;
  }
}
.keywordInput {
  height: 34px;
  padding: 5px 30px;
  border-color: transparent;
  @media screen and (max-width: 769px) {
    width: 100%;
  }
}
// .header__search {
//   @media screen and (max-width: 769px) {
//     display: none;
//   }
// }
</style>

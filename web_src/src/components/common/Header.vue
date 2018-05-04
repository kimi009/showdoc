<template>
  <div class="frame_header">
    <div class="header">
      <div class="psb_header_left">
        <img class="img" src="static/logo/psb_logo.png" alt="logo">
      </div>
      <div class="psb_header_right">
        <el-input class="psb_header_search" @keyup.enter.native="input_keyword" :placeholder="$t('input_keyword')" v-model="keyword">
        </el-input>
        <span class="contact">联系我们</span>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      keyword: "",
      msg: "头部"
    };
  },
  methods: {
    input_keyword() {
      if (typeof this.$parent.search_item == "function") {
        this.$parent.search_item(this.keyword);
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.frame_header {
  position: fixed;
  width: 100%;
  background: #fff;
  border-bottom: 1px solid #e4e6e9;
  height: 54px;
  z-index: 10;
}
.header {
  position: relative;
  padding: 0 30px;
}
.psb_header_right {
  position: absolute;
  right: 30px;
  font-size: 13px;
  line-height: 54px;
  color: #787878;
}
.psb_header_left {
  float: left;
  height: 54px;
}
.psb_header_left > .img {
  height: 26px;
  margin-top: 14px;
}
.psb_header_search {
  right: 20px;
}
.el-input {
  width: 300px;
}
.contact {
  color: #0c4c7f;
  text-decoration: none;
}
</style>

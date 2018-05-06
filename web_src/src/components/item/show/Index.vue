<template>
  <div>
    <Header></Header>
    <!-- 展示常规项目 -->
    <ShowRegularItem :item_info="item_info" :search_item="search_item" v-if="item_info && item_info.item_type == 1 ">

    </ShowRegularItem>

    <!-- 展示单页项目 -->
    <ShowSinglePageItem :item_info="item_info" v-if="item_info && item_info.item_type == 2 ">

    </ShowSinglePageItem>

    <Footer> </Footer>

  </div>
</template>



<script>
import ShowRegularItem from "@/components/item/show/show_regular_item/Index";
import ShowSinglePageItem from "@/components/item/show/show_single_page_item/Index";
export default {
  data() {
    return {
      item_info: ""
    };
  },
  components: {
    ShowRegularItem,
    ShowSinglePageItem
  },
  methods: {
    //获取菜单
    get_item_menu(keyword) {
      if (!keyword) {
        keyword = "";
      }
      var that = this;
      var loading = that.$loading();
      var item_id = this.$route.params.item_id ? this.$route.params.item_id : 0;
      var page_id = this.$route.query.page_id ? this.$route.query.page_id : 0;

      var url = DocConfig.server + "/api/item/info";

      var params = new URLSearchParams();
      params.append("item_id", item_id);
      params.append("keyword", keyword);
      if (!that.keyword) {
        params.append("default_page_id", page_id);
      }
      // let testData =
      //   '{"item_id":"1","item_domain":"","is_archived":"0","default_page_id":"0","default_cat_id2":0,"default_cat_id3":0,"unread_count":null,"item_type":1,"menu":{"pages":[],"catalogs":[{"cat_id":"3","cat_name":"开发前必读","item_id":"1","s_number":"1","addtime":"1524800764","parent_cat_id":"0","level":"2","pages":[{"page_id":"2","author_uid":"2","cat_id":"3","page_title":"概述","addtime":"1525331430"},{"page_id":"28","author_uid":"2","cat_id":"3","page_title":"开始开发","addtime":"1525332441"},{"page_id":"3","author_uid":"2","cat_id":"3","page_title":"更新日志","addtime":"1525314955"},{"page_id":"5","author_uid":"2","cat_id":"3","page_title":"全局返回码","addtime":"1525315000"}],"catalogs":[]},{"cat_id":"6","cat_name":"身份验证","item_id":"1","s_number":"10","addtime":"1524803491","parent_cat_id":"0","level":"2","pages":[{"page_id":"29","author_uid":"1","cat_id":"6","page_title":"网页授权登陆","addtime":"1525400388"},{"page_id":"11","author_uid":"2","cat_id":"6","page_title":"接口调用验证","addtime":"1525321120"},{"page_id":"30","author_uid":"2","cat_id":"6","page_title":"票税宝账号注册","addtime":"1525347093"},{"page_id":"31","author_uid":"2","cat_id":"6","page_title":"票税宝账号登陆","addtime":"1525347131"}],"catalogs":[]},{"cat_id":"18","cat_name":"发票管理H5网页","item_id":"1","s_number":"20","addtime":"1525313894","parent_cat_id":"0","level":"2","pages":[{"page_id":"7","author_uid":"2","cat_id":"18","page_title":"发票列表","addtime":"1525347594"},{"page_id":"14","author_uid":"2","cat_id":"18","page_title":"添加发票","addtime":"1525347496"},{"page_id":"16","author_uid":"2","cat_id":"18","page_title":"选择发票","addtime":"1525347433"},{"page_id":"15","author_uid":"2","cat_id":"18","page_title":"查看发票","addtime":"1525347444"},{"page_id":"17","author_uid":"2","cat_id":"18","page_title":"发票抬头","addtime":"1525347458"}],"catalogs":[]},{"cat_id":"19","cat_name":"发票管理WEB网页","item_id":"1","s_number":"30","addtime":"1525313959","parent_cat_id":"0","level":"2","pages":[{"page_id":"20","author_uid":"2","cat_id":"19","page_title":"发票列表","addtime":"1525347640"},{"page_id":"22","author_uid":"2","cat_id":"19","page_title":"添加发票","addtime":"1525347771"},{"page_id":"24","author_uid":"2","cat_id":"19","page_title":"选择发票","addtime":"1525347782"},{"page_id":"23","author_uid":"2","cat_id":"19","page_title":"查看发票","addtime":"1525347789"}],"catalogs":[]},{"cat_id":"12","cat_name":"第三方开放接口","item_id":"1","s_number":"40","addtime":"1524881237","parent_cat_id":"0","level":"2","pages":[{"page_id":"8","author_uid":"2","cat_id":"12","page_title":"导入企业项目","addtime":"1525342247"},{"page_id":"9","author_uid":"2","cat_id":"12","page_title":"导入费用类型","addtime":"1525342255"},{"page_id":"10","author_uid":"2","cat_id":"12","page_title":"获取已报销发票","addtime":"1525342269"},{"page_id":"21","author_uid":"2","cat_id":"12","page_title":"同步发票报销状态","addtime":"1525342352"},{"page_id":"25","author_uid":"2","cat_id":"12","page_title":"获取发票详情","addtime":"1525342384"}],"catalogs":[]}]},"is_login":true,"ItemPermn":true,"ItemCreator":false}';
      // let json = JSON.parse(testData);
      // if (json.default_page_id <= 0) {
      //   if (json.menu.pages[0]) {
      //     json.default_page_id = json.menu.pages[0].page_id;
      //   }
      // }
      // that.item_info = json;
      // document.title = "ShowDoc--" + that.item_info.item_name;
      // if (json.unread_count > 0) {
      //   that.$message({
      //     showClose: true,
      //     duration: 10000,
      //     dangerouslyUseHTMLString: true,
      //     message: '<a href="#/notice/index">你有新的未读消息，点击查看</a>'
      //   });
      // }
      that.axios.post(url, params).then(function(response) {
        loading.close();
        if (response.data.error_code === 0) {
          var json = response.data.data;
          console.log(`json=${JSON.stringify(json)}`)
          if (json.default_page_id <= 0) {
            if (json.menu.pages[0]) {
              json.default_page_id = json.menu.pages[0].page_id;
            }
          }
          that.item_info = json;
          document.title = "ShowDoc--" + that.item_info.item_name;
          if (json.unread_count > 0) {
            that.$message({
              showClose: true,
              duration: 10000,
              dangerouslyUseHTMLString: true,
              message: '<a href="#/notice/index">你有新的未读消息，点击查看</a>'
            });
          }
        } else if (
          response.data.error_code === 10307 ||
          response.data.error_code === 10303
        ) {
          //需要输入密码
          that.$router.replace({
            path: "/item/password/" + item_id,
            query: {
              page_id: page_id,
              redirect: that.$router.currentRoute.fullPath
            }
          });
        } else {
          that.$alert(response.data.error_message);
        }
      });
      //设置一个最长关闭时间
      setTimeout(() => {
        loading.close();
      }, 500);
    },
    search_item(keyword) {
      this.item_info = "";
      this.get_item_menu(keyword);
    }
  },
  mounted() {
    this.get_item_menu();
  },
  beforeDestroy() {
    this.$message.closeAll();
    /*去掉添加的背景色*/
    document.body.removeAttribute("class", "grey-bg");
    document.title = "ShowDoc";
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.el-container {
  position: relative;
  top: 30px;
}
.el-aside {
  top: 54px;
}
.page-bar {
  position: relative;
  top: 30px;
}
.container-narrow {
  top: 80px;
}
.el-card {
  top: 330px;
}
</style>

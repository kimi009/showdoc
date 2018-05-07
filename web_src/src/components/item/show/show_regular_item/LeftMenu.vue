<template>
  <div class="hello">
    <div class="left_menu">
      <div class="new-bar" v-if="item_info.ItemPermn && item_info.is_archived < 1 ">
        <el-tooltip class="item" effect="dark" :content="$t('new_page')" placement="left">
          <i class="el-icon-plus" @click="new_page"></i>
        </el-tooltip>
        <el-tooltip class="item" effect="dark" :content="$t('new_catalog')" placement="right">
          <i class="el-icon-message" @click="mamage_catalog"></i>
        </el-tooltip>
      </div>
      <div v-if="menu.pages.length " v-for="(page ,index) in menu.pages" :index="page.page_id" :key="page.page_id">
        <p class="catalog-title">{{page.page_title}}</p>
      </div>
      <div class="catalog" v-if="menu.catalogs.length" v-for="(catalog2 ,catalog_index) in menu.catalogs" :index="catalog2.cat_id" :key="catalog2.cat_id">
        <p class="catalog-page-title catalog2-page-title">{{catalog2.cat_name}}</p>
        <div v-if="catalog2.pages" v-for="(page2 ,page2_index) in catalog2.pages" :key="page2.page_id">
          <p class="catalog-title catalog2-title" :class="currentId == page2.page_id ? 'catalog-actived-title' : 'catalog-normal-title'" @click="selectItem(page2.page_id)">{{page2.page_title}}</p>
        </div>
        <div v-if="catalog2.catalogs.length" v-for="(catalog3 ,catalog_index3) in catalog2.catalogs" :index="catalog3.cat_id" :key="catalog3.cat_id">
          <p class="catalog-page-title catalog3-page-title">{{catalog3.cat_name}}</p>
          <div v-if="catalog3.pages" v-for="(page3 ,page3_index) in catalog3.pages" :index="page3.page_id" :key="page3.page_id">
            <p class="catalog-title catalog3-title" :class="currentId == page3.page_id ? 'catalog-actived-title' : 'catalog-normal-title'" @click="selectItem(page3.page_id)">{{page3.page_title}}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- <el-menu @select="select_menu" background-color="#fafafa" text-color="" active-text-color="#008cff" :default-active="item_info.default_page_id" :default-openeds='openeds' :unique-opened='false'>

          <div class="new-bar" v-if="item_info.ItemPermn && item_info.is_archived < 1 ">
            <el-tooltip class="item" effect="dark" :content="$t('new_page')" placement="left">
              <i class="el-icon-plus" @click="new_page"></i>
            </el-tooltip>
            <el-tooltip class="item" effect="dark" :content="$t('new_catalog')" placement="right">
              <i class="el-icon-message" @click="mamage_catalog"></i>
            </el-tooltip>
          </div> -->

    <!-- 一级页面 -->
    <!-- <el-menu-item v-if="menu.pages.length " v-for="(page ,index) in menu.pages" :index="page.page_id" :key="page.page_id">
            <i style="margin-left:30px;"></i>
            {{page.page_title}}
          </el-menu-item> -->

    <!-- 目录开始 -->
    <!-- <el-submenu v-if="menu.catalogs.length" v-for="(catalog2 ,catalog_index) in menu.catalogs" :index="catalog2.cat_id" :key="catalog2.cat_id"> -->
    <!-- 二级目录名 -->
    <!-- <template slot="title">
              <i class="el-icon-message"></i>
              {{catalog2.cat_name}}
            </template> -->

    <!-- 二级目录的页面 -->
    <!-- <el-menu-item-group v-if="catalog2.pages" v-for="(page2 ,page2_index) in catalog2.pages" :key="page2.page_id">
              <el-menu-item :index="page2.page_id">{{page2.page_title}}</el-menu-item>
            </el-menu-item-group> -->

    <!-- 二级目录下的三级目录 -->
    <!-- <el-submenu v-if="catalog2.catalogs.length" v-for="(catalog3 ,catalog_index3) in catalog2.catalogs" :index="catalog3.cat_id" :key="catalog3.cat_id">
              <template slot="title">{{catalog3.cat_name}}</template> -->
    <!-- 三级目录的页面 -->
    <!-- <el-menu-item v-if="catalog3.pages" v-for="(page3 ,page3_index) in catalog3.pages" :index="page3.page_id" :key="page3.page_id">{{page3.page_title}}</el-menu-item>
            </el-submenu>

          </el-submenu>

        </el-menu> -->
  </div>
</template>


<script>
import Editormd from "@/components/common/Editormd";
export default {
  props: {
    get_page_content: "",
    item_info: "",
    search_item: ""
  },
  data() {
    return {
      // keyword:'',
      openeds: [],
      menu: "",
      currentId: ""
    };
  },
  components: {
    Editormd
  },
  methods: {
    selectItem(pageId) {
      this.currentId = pageId;
      this.select_menu(pageId);
    },
    //选中菜单的回调
    select_menu(index, indexPath) {
      console.log(`index=${index} indexPath=${indexPath}`);
      this.change_url(index);
      this.get_page_content(index);
    },
    new_page() {
      var url = "/page/edit/" + this.item_info.item_id + "/0";
      this.$router.push({ path: url });
    },

    mamage_catalog() {
      var url = "/catalog/" + this.item_info.item_id;
      this.$router.push({ path: url });
    },

    //改变url
    change_url(page_id) {
      var base_url = "";
      var item_domain = "";
      var domain = this.item_info.item_domain
        ? this.item_info.item_domain
        : this.item_info.item_id;
      this.$router.replace({
        path: "/" + domain,
        query: { page_id: page_id }
      });
    }

    // input_keyword(){
    //   this.search_item(this.keyword);
    // }
  },
  mounted() {
    var that = this;
    this.menu = this.item_info.menu;
    var item_info = this.item_info;
    this.$nextTick(() => {
      if (that.$route.query.page_id) {
        that.selectItem(that.$route.query.page_id);
      } else if (
        that.menu &&
        that.menu.catalogs &&
        that.menu.catalogs[0] &&
        that.menu.catalogs[0].pages &&
        that.menu.catalogs[0].pages[0]
      ) {
        console.log(`page1=${JSON.stringify(that.menu.catalogs[0].pages[0])}`);
        that.selectItem(that.menu.catalogs[0].pages[0].page_id);
      }
    });

    //默认展开页面
    // if (item_info.default_page_id > 0) {
    //   that.select_menu(item_info.default_page_id);
    //   if (item_info.default_cat_id3) {
    //     that.openeds = [
    //       item_info.default_cat_id3,
    //       item_info.default_cat_id2,
    //       item_info.default_page_id
    //     ];
    //   } else if (item_info.default_cat_id2) {
    //     that.openeds = [item_info.default_cat_id2, item_info.default_page_id];
    //   }
    // } else {
    //   that.select_menu(2);
    // }
    // that.openeds = ["3", "6", "18", "19"];
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.left_menu {
  padding-bottom: 100px;
}
.el-header {
  color: #333;
  line-height: 60px;
}

.el-aside {
  color: #333;
  position: fixed;
  height: calc(100% - 20px);
}
.el-input-group__append button.el-button {
  background-color: #ffffff;
}
.new-bar {
  margin-left: 190px;
  font-size: 20px;
  margin-top: 10px;
  margin-bottom: 5px;
}
.new-bar i {
  cursor: pointer;
}

.new-bar i:first-child {
  margin-right: 15px;
}
.el-menu {
  border-right: none;
  background-color: #edeff2 !important;
}

.catalog {
  margin-bottom: 14px;
}
.catalog-page-title {
  margin: 0px;
  font-weight: 700;
  line-height: 32px;
  color: #999;
  cursor: default;
  padding: 0 30px;
}
.catalog2-page-title {
  font-size: 16px;
}
.catalog3-page-title {
  font-size: 14px;
  /* margin-left: 16px; */
}
.catalog-title {
  margin: 0px;
  line-height: 30px;
  color: #000;
  padding: 0 30px;
  cursor: pointer;
}
.catalog2-title {
  font-size: 14px;
  /* margin-left: 16px; */
}
.catalog3-title {
  font-size: 14px;
  margin-left: 20px;
}
.catalog-actived-title {
  background-color: #4a77ac;
  color: #fff;
}
.catalog-normal-title {
  background-color: #edeff2;
  color: #000;
}
</style>
<style>
.el-menu,
.el-submenu,
.el-submenu__title,
.el-menu-item,
.el-menu .el-menu--inline {
  background-color: #edeff2 !important;
}
#left-side::-webkit-scrollbar {
  width: 6px;
  height: 1px;
  background: rgba(0, 0, 0, 0.1);
}
#left-side::-webkit-scrollbar:hover {
  background: rgba(0, 0, 0, 0.2);
}
#left-side::-webkit-scrollbar-thumb {
  /* background: rgba(0, 0, 0, 0.3); */
  -webkit-border-radius: 6px;
  -moz-border-radius: 6px;
  -ms-border-radius: 6px;
  -o-border-radius: 6px;
  border-radius: 6px;
}
#left-side::-webkit-scrollbar-thumb:hover {
  -webkit-box-shadow: inset 1px 1px 1px rgba(0, 0, 0, 0.25);
  /* Webkit browsers */
  -moz-box-shadow: inset 1px 1px 1px rgba(0, 0, 0, 0.25);
  /* Firefox */
  -ms-box-shadow: inset 1px 1px 1px rgba(0, 0, 0, 0.25);
  /* IE9 */
  -o-box-shadow: inset 1px 1px 1px rgba(0, 0, 0, 0.25);
  /* Opera(Old) */
  box-shadow: inset 1px 1px 1px rgba(0, 0, 0, 0.25);
  /* IE9+, News */
  background-color: rgba(0, 0, 0, 0.4);
}
</style>


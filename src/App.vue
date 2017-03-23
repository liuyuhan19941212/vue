<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
    	<div class="tab-item">
    		<router-link to="/goods">商品</router-link>
    	</div>
    	<div class="tab-item">
    	  <router-link to="/ratings">评价</router-link></div>
    	<div class="tab-item">
    	  <router-link to="/seller">商家</router-link>
    	</div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
import header from './components/header/header.vue';
import {urlParse} from './common/js/util';

    export default {
      data() {
        return {
          seller: {
            id: (() => {
              let queryParam = urlParse();
              return queryParam.id;
            })()
          }
          };
    },

    created() {
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
      response = response.body;
          if (response.errno === 0) {
          // this.seller = response.data;
         //  相当于 extend方法 扩展  es6语法   vue推荐的给对象扩展属性方法
            this.seller = Object.assign({}, this.seller, response.data);
          }
      });
    },
  components: {
    'v-header': header
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"
  .tab{
 	 display: flex;
   width:100%;
   height:40px;
   line-height:40px;
  };
  .tab-item{
  	   flex:1;
       text-align:center;
  };
  .active{
  	color: #f01414;
  };
</style>

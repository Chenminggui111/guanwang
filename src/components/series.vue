<template>
  <div>
    <div class="content">
      <h1 class="series_title">{{$t('product.name')}}</h1>
      <div class="series_item" v-if="this.isPc()==1">
        <div class="item" v-for="item in list" v-bind:key="item.id">
          <router-link :to="{ name: 'SeriesType', query: { id: item.id }}">
            <div class="mask">
              <div class="mask_text">{{item.title}}</div>
            </div>
            <img v-bind:src="item.image" alt />
          </router-link>
        </div>
      </div>
      <div class="series_item_mobile"  v-if="this.isPc()==2">
        <div class="item" v-for="item in list" v-bind:key="item.id">
          <router-link :to="{ name: 'SeriesType', query: { id: item.id }}">
            <div class="mask">
              <div class="mask_text">{{item.title}}</div>
            </div>
            <img v-bind:src="item.image" alt />
          </router-link>
        </div>
      </div>
    </div>
    <!-- <el-row class="content">
      <nav-component></nav-component>
      <el-row class="series_title">{{$t('product.name')}}</el-row>
      <el-row class="series_pic" type="flex" justify="center" :gutter="10">
        <el-col class="series_img" :span="12" v-for="item in list" v-bind:key="item.id">
          <router-link :to="{ name: 'SeriesType', params: { id: item.id }}">
            <div class="mask">
              <div class="mask_text">{{item.title}}</div>
            </div>
            <img v-bind:src="item.image" alt />
          </router-link>
        </el-col>
      </el-row>
    </el-row>-->
  </div>
</template>
<script>
import { requestTypes } from "../api/api";
export default {
  name: "Series",

  data() {
    return {
      count: 0,
      limit: 10,
      currentPage: 1,
      list: []
    };
  },
  methods: {
    getList() {
      var language = this.$i18n.locale == "zh" ? "1" : "2";
      var allParams =
        "?page=" +
        this.currentPage +
        "&limit=" +
        this.limit +
        "&language=" +
        language +
        "&sort=1";
      requestTypes(allParams).then(res => {
        this.list = res.data.data;
        this.count = res.data.count;
      });
    }
  },
  created() {
    // 控制是否显示导航栏
    this.$bus.on("ChangeLocation", val => {
      this.getList();
    });
  },
  mounted: function() {
    this.getList();
  }
};
</script>
<style scoped lang="less">
.content {
  width: 96rem;
  padding: 0 15rem;
  margin-top: 5.1rem;
  .series_title {
    width: 100%;
    text-align: center;
    color: #86837a;
    font-size: 2.6rem;
    font-weight: bold;
  }
  .series_item {
    margin-top: 5rem;
    .item {
      display: inline-block;
      width: 32rem;
      height: 32rem;
      margin-right: 1rem;
      margin-bottom: 1rem;
      position: relative;

      &:nth-child(even) {
        margin-right: 0;
      }
      &:hover .mask {
        display: block;
        opacity: 0.5;
        animation: fade 2s;
      }
      .mask {
        transition: all 2s;
        display: none;
        width: 100%;
        height: 100%;
        background: #000;
        position: absolute;
        opacity: 0.6;
        .mask_text {
          width: 100%;
          height: 100%;
          display: block;
          font-size: 2.2rem;
          font-weight: bolder;
          text-align: center;
          line-height: 30rem;
          color: #fff;
        }
      }
      img {
        width: 100%;
        height: 100%;
      }
    }
  }
  .series_item_mobile {
    margin-top: 5rem;
    .item {
      display: inline-block;
      width: 32rem;
      height: 32rem;
      margin-right: 1rem;
      margin-bottom: 1rem;
      position: relative;
&:nth-child(even) {
        margin-right: 0;
      }
      .mask {
        // transition: all 2s;
        // display: none;
        width: 100%;
        height: 100%;
        // background: #000;
        position: absolute;
        // opacity: 0.6;
        .mask_text {
          width: 100%;
          height: 100%;
          display: block;
          font-size: 2.2rem;
          font-weight: bolder;
          text-align: center;
          line-height: 30rem;
          color: #fff;
        }
      }
      img {
        width: 100%;
        height: 100%;
      }
    }
  }
}
@keyframes fade {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 0.5;
  }
}
</style>
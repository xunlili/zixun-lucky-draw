<!--
 * @Author: ken yang
 * @Date: 2022-01-11 12:30:50
 * @LastEditTime: 2022-01-11 15:27:58
 * @LastEditors: ken yang
 * @Description: 
 * @FilePath: \lucky-draw\src\components\Publicity.vue
-->
<template>
  <div class="c-Publicity">
    <el-carousel
      height="50px"
      :autoplay="true"
      indicator-position="none"
      arrow="never"
      :interval="3000"
    >
      <el-carousel-item v-for="item in message" :key="item.key">
        <div class="item" :class="{ actiname: item.key === 0 }">
          <span v-if="item.title" class="title"> {{ item.title }}</span>
          <span v-if="item.value" class="value">
            {{ item.value }}
          </span>
        </div>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>
<script>
import { conversionCategoryName } from '@/helper/index';

export default {
  name: 'Publicity',
  computed: {
    config() {
      return this.$store.state.config;
    },
    result() {
      return this.$store.state.result;
    },
    message() {
      const { result, config } = this;
      const fields = Object.keys(config);

      let message = [{ key: 0, title: config.name }];
      fields.forEach((item, index) => {
        let label = conversionCategoryName(item);
        if (result[item] && config[item] > 0) {
          message.push({
            key: index + 1,
            title: `${label}抽奖结果:`,
            value: `${
              result[item].length > 0 ? this.handle(result[item]) : '暂未抽取'
            }`
          });
        }
      });

      return message;
    }
  },
  methods: {
    handle(arr) {
      let narr = arr.map(element => {
        return this.handleCodeToName(element);
      });
      return narr.join('、');
    },
    handleCodeToName(code) {
      let list = this.$store.state.list;
      let targetItem = list.filter(item => {
        return item.key === code;
      });
      return targetItem[0]?.name || code;
    }
  }
};
</script>
<style lang="scss">
.c-Publicity {
  height: 100%;
  // width: 1000px;
  background-color: rgba(255, 255, 255, 0.1);
  margin: 0 auto;
  position: relative;
  overflow: hidden;
  .el-carousel {
    width: 80vw;
    margin: 0 auto;
  }
  .item {
    text-align: center;
    color: #fff;
    font-size: 16px;
    .title {
      color: #ccc;
    }
    .value {
      margin-left: 10px;
    }
    &.actiname {
      .title {
        color: #fff;
        font-size: 20px;
      }
    }
  }
}
</style>

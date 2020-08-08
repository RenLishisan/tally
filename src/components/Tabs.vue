<template>
  <ul :class="{[classPrefix+'-tabs']:classPrefix}" class="tabs">
    <li :class="liClass(item)" :key="item.value" class="tabs-item"
        @click="select(item)" v-for="item in dataSource">{{ item.text }}
    </li>
  </ul>
</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component, Prop} from 'vue-property-decorator';

  type DataSourceItem = { text: string; value: string }

  @Component
  export default class Tabs extends Vue {
    @Prop({required: true, type: Array})
    dataSource!: DataSourceItem[];
    @Prop(String) readonly value!: string;
    @Prop(String) classPrefix?: string;
    @Prop({type: String, default: '64px'})
    height!: string;

    liClass(item: DataSourceItem) {
      return {
        [this.classPrefix + '-tabs-item']: this.classPrefix,
        selected: item.value === this.value
      };
    }

    select(item: DataSourceItem) {
      this.$emit('update:value', item.value);
    }
  }
</script>

<style lang="scss" scoped>
  .tabs {
    display: flex;
    text-align: center;
    font-size: 24px;
    border-radius: 0 0 12px 12px;
    background: linear-gradient(145deg, #c0ccd4, #e4f3fd);
    box-shadow:  6px 6px 13px 13px #b9c5cd;

    &-item {
      width: 50%;
      height: 64px;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;


      &.selected::after {
        content: '';
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 4px;
        background: #333;
      }
    }
  }
</style>
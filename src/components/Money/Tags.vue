<template>
  <div class="tags">
    <div class="new">
      <button @click="createTag">新增加标签</button>
    </div>
    <ul class="current">
      <li :class="{selected: selectedTags.indexOf(tag)>=0}" :key="tag.id"
          @click="toggle(tag)"
          v-for="tag in tagList">{{ tag.name }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component, Prop} from 'vue-property-decorator';
  import {mixins} from 'vue-class-component';
  import TagHelper from '@/mixins/TagHelper';

  @Component
  export default class Tags extends mixins(TagHelper) {
    selectedTags: string[] = [];

    get tagList() {
      return this.$store.state.tagList;
    }

    created() {
      this.$store.commit('fetchTags');
    }

    toggle(tag: string) {
      const index = this.selectedTags.indexOf(tag);
      if (index >= 0) {
        this.selectedTags.splice(index, 1);
      } else {
        this.selectedTags.push(tag);
      }
      this.$emit('update:value', this.selectedTags);
    }
  }
</script>

<style lang="scss" scoped>
  .tags {
    font-size: 14px;
    margin-bottom: -14px;
    padding: 16px;
    flex-grow: 1;
    display: flex;
    flex-direction: column-reverse;
    background: linear-gradient(145deg, #c0ccd4, #e4f3fd);
    box-shadow: 6px 6px 13px #b9c5cd,
    -6px -6px 13px #f1ffff;


    > .current {
      display: flex;
      flex-wrap: wrap;


      > li {
        $bg: #d9d9d9;
        background: $bg;
        $h: 30px;
        height: $h;
        line-height: $h;
        border-radius: $h/2;
        padding: 0 16px;
        margin-right: 12px;
        margin-top: 4px;
        border-radius: 6px;
        background: #d5e3ec;
        box-shadow:  2px 2px 5px #b5c1c9,
        -2px -2px 5px #f5ffff;


        &.selected {
          border-radius: 6px;
          background: #d5e3ec;
          box-shadow: inset 5px 5px 5px #a8b3ba,
          inset -5px -5px 5px #ffffff;
        }
      }
    }

    > .new {
      padding-top: 16px;


      button {
        background: transparent;
        border: none;
        color: #999;
        border-bottom: 1px solid;
        padding: 0 3px;
      }
    }
  }
</style>
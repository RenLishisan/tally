<template>
  <layout>
    <div class="tags">
      <router-link :key="tag.id" :to="`/labels/edit/${tag.name}`" class="tag"
                   v-for="tag in tags">
        <span>{{ tag.name }}</span>
        <Icon name="bq"/>
      </router-link>
    </div>
    <div class="createTag-wrapper">
      <Button @click="createTag"
              class="createTag">新建标签
      </Button>
    </div>
  </layout>
</template>

<script lang="ts">
  import {Component} from 'vue-property-decorator';
  import Button from '@/components/Button.vue';
  import {mixins} from 'vue-class-component';
  import TagHelper from '@/mixins/TagHelper';

  @Component({
    components: {Button},
  })
  export default class Labels extends mixins(TagHelper) {
    get tags() {
      return this.$store.state.tagList;
    }

    beforeCreate() {
      this.$store.commit('fetchTags');
    }
  }
</script>

<style lang="scss" scoped>
  .tags {
    font-size: 16px;
    background: rgb(213, 227, 236);
    display: flex;
    flex-wrap: wrap;
    justify-content: center;


    > .tag {
      top: 10px;
      height: 80px;
      width: 70px;
      margin-right: 8px;
      position: relative;
      display: flex;
      flex-direction: column-reverse;
      justify-content: center;
      align-items: center;
      margin-top: 5px;
      border-radius: 12px;
      background: #d5e3ec;
      box-shadow:  8px 8px 16px #c0ccd4,
      -8px -8px 16px #eafaff;
       > svg {
          width: 50px;
          height: 50px;
          color: #87CEFA;

        }

    }
    .tag:active{
      border-radius: 12px;
      background: #d5e3ec;
      box-shadow: inset 8px 8px 16px #c0ccd4,
      inset -8px -8px 16px #eafaff;
    }
  }

  .createTag {
    border: none;
    height: 40px;
    padding: 0 16px;
    border-radius: 12px;
    background: #d5e3ec;
    box-shadow:  8px 8px 15px #c0ccd4,
    -8px -8px 15px #eafaff;
    &-wrapper {
      text-align: center;
      padding: 16px;
      margin-top: 44-16px;
    }

  }
  .createTag:active{
    border: none;
    height: 40px;
    padding: 0 16px;
    border-radius: 12px;
    background: #d5e3ec;
    box-shadow: inset 8px 8px 15px #c0ccd4,
    inset -8px -8px 15px #eafaff;
  }
</style>
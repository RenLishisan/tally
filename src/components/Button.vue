<template>
  <button @click="$emit('click',$event)"
          class="button">
    <slot/>
  </button>
</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component} from 'vue-property-decorator';

  @Component
  export default class Button extends Vue {

  }
</script>

<style lang="scss" scoped>
  .button {
    color: white;
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
  .button:active{
    color: white;
    border: none;
    height: 40px;
    padding: 0 16px;
    border-radius: 12px;
    background: #d5e3ec;
    box-shadow: inset 8px 8px 15px #c0ccd4,
    inset -8px -8px 15px #eafaff;
  }
</style>
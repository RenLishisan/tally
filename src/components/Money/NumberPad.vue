<template>
  <div class="numberPad">
    <div class="output">{{ output }}</div>
    <div class="buttons">
      <button @click="inputContent">1</button>
      <button @click="inputContent">2</button>
      <button @click="inputContent">3</button>
      <button @click="remove">删除</button>
      <button @click="inputContent">4</button>
      <button @click="inputContent">5</button>
      <button @click="inputContent">6</button>
      <button @click="clear">清空</button>
      <button @click="inputContent">7</button>
      <button @click="inputContent">8</button>
      <button @click="inputContent">9</button>
      <button @click="ok" class="ok">OK</button>
      <button @click="inputContent" class="zero">0</button>
      <button @click="inputContent">.</button>
    </div>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component, Prop} from 'vue-property-decorator';

  @Component
  export default class NumberPad extends Vue {
    @Prop(Number) readonly value!: number;
    output = this.value.toString();

    inputContent(event: MouseEvent) {
      const button = (event.target as HTMLButtonElement);
      const input = button.textContent!;
      if (this.output.length === 16) { return; }
      if (this.output === '0') {
        if ('0123456789'.indexOf(input) >= 0) {
          this.output = input;
        } else {
          this.output += input;
        }
        return;
      }
      if (this.output.indexOf('.') >= 0 && input === '.') {return;}
      this.output += input;
    }

    remove() {
      if (this.output.length === 1) {
        this.output = '0';
      } else {
        this.output = this.output.slice(0, -1);
      }
    }

    clear() {
      this.output = '0';
    }

    ok() {
      setTimeout(()=>{
        const number = parseFloat(this.output);
        this.$emit('update:value', number);
        this.$emit('submit', number);
        this.output = '0';
      },0)
      }
  }
</script>

<style lang="scss" scoped>
  @import "~@/assets/style/helper.scss";

  .numberPad {
    .output {
      @extend %clearFix;
      @extend %innerShadow;
      font-size: 36px;
      font-family: Consolas, monospace;
      padding: 9px 16px;
      text-align: right;
      height: 72px;
      background: linear-gradient(145deg, #c0ccd4, #e4f3fd);
      box-shadow:  6px 6px 13px #b9c5cd,
      -6px -6px 13px #f1ffff

    }

    .buttons {
      @extend %clearFix;

      > button {
        width: 25%;
        height: 64px;
        float: left;
        border: none;

        &.ok {
          height: 64*2px;
          float: right;
        }

        &.zero {
          width: 25*2%;
        }


        &:nth-child(1) {
          border-radius: 12px;
          background: #d5e3ec;
          box-shadow:  5px 5px 8px #bbc8d0,
          -5px -5px 8px #effeff;
        }
        &:nth-child(1):active{
          border-radius: 12px;
          background: #d5e3ec;
          box-shadow: inset 6px 6px 7px #becad2,
          inset -6px -6px 7px #ecfcff;
        }

        &:nth-child(2), &:nth-child(5) {
          border-radius: 12px;
          background: #d5e3ec;
          box-shadow:  5px 5px 8px #bbc8d0,
          -5px -5px 8px #effeff;
        }
        &:nth-child(2):active, &:nth-child(5):active{
          border-radius: 12px;
          background: #d5e3ec;
          box-shadow: inset 6px 6px 7px #becad2,
          inset -6px -6px 7px #ecfcff;
        }

        &:nth-child(3), &:nth-child(6), &:nth-child(9) {
          border-radius: 12px;
          background: #d5e3ec;
          box-shadow:  5px 5px 8px #bbc8d0,
          -5px -5px 8px #effeff;
        }
        &:nth-child(3):active, &:nth-child(6):active, &:nth-child(9):active{
          border-radius: 12px;
          background: #d5e3ec;
          box-shadow: inset 6px 6px 7px #becad2,
          inset -6px -6px 7px #ecfcff;
        }


        &:nth-child(4), &:nth-child(7), &:nth-child(10), &:nth-child(13) {
          border-radius: 12px 0 0 12px;
          background: #d5e3ec;
          box-shadow:  5px 5px 8px #bbc8d0,
          -5px -5px 8px #effeff;
        }
        &:nth-child(4):active, &:nth-child(7):active, &:nth-child(10):active, &:nth-child(13):active{
          border-radius: 12px 0 0 12px;
          background: #d5e3ec;
          box-shadow: inset 6px 6px 7px #becad2,
          inset -6px -6px 7px #ecfcff;
        }

        &:nth-child(8), &:nth-child(11), &:nth-child(13) {
          border-radius: 12px 0 0 12px;
          background: #d5e3ec;
          box-shadow:  5px 5px 8px #bbc8d0,
          -5px -5px 8px #effeff;
        }
        &:nth-child(8):active, &:nth-child(11):active, &:nth-child(13):active{
          border-radius: 12px 0 0 12px;
          background: #d5e3ec;
          box-shadow: inset 6px 6px 7px #becad2,
          inset -6px -6px 7px #ecfcff;
        }

        &:nth-child(14) {
          border-radius: 12px 0 0 12px;
          background: #d5e3ec;
          box-shadow:  5px 5px 8px #bbc8d0,
          -5px -5px 8px #effeff;
        }
        &:nth-child(14):active{
          border-radius: 12px 0 0 12px;
          background: #d5e3ec;
          box-shadow: inset 6px 6px 7px #becad2,
          inset -6px -6px 7px #ecfcff;
        }

        &:nth-child(12) {
          border-radius: 12px 0 0 12px;
          background: #d5e3ec;
          box-shadow:  5px 5px 8px #bbc8d0,
          -5px -5px 8px #effeff;
        }
        &:nth-child(12):active{
          border-radius: 12px 0 0 12px;
          background: #d5e3ec;
          box-shadow: inset 6px 6px 7px #becad2,
          inset -6px -6px 7px #ecfcff;
        }
      }
    }
  }

</style>
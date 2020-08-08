<template>
    <nav>
        <router-link active-class="selected" class="item" to="/money">
            <Icon name="money"/>
            记账
        </router-link>
        <router-link active-class="selected" class="item" to="/labels">
            <Icon name="labels"/>
            标签</router-link>
        <router-link active-class="selected" class="item" to="/statistics">
            <Icon name="statistics"/>
            统计</router-link>
    </nav>
</template>

<script lang="ts">

    const importAll = (requireContext: __WebpackModuleApi.RequireContext) => requireContext.keys().forEach(requireContext);
    try {
        importAll(require.context('../assets/icons', true, /\.svg$/));
    } catch (error) {
        console.log(error);
    }


    export default {
        name: 'Nav'
    };
</script>

<style lang="scss" scoped>
  @import "~@/assets/style/helper.scss";
  nav {
    @extend %outerShadow;
    display: flex;
    flex-direction: row;
    font-size: 12px;
    border-radius: 12px 12px 0 0;
    background: #d5e3ec;
    box-shadow:  5px 5px 8px #bbc8d0,
    -5px -5px 8px #effeff;
    z-index: 10;


    > .item {
      padding: 2px 0;
      width: 33.33333%;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;

      .icon {
        width: 32px;
        height: 32px;
      }
    }
    > .item.selected{
      color: $color-highlight;
    }
  }
</style>
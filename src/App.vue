<template>
<div id="app">
    <router-view/>
</div>
</template>

<style lang="scss">
@import "~@/assets/style/reset.scss";
@import "~@/assets/style/helper.scss";
    body{
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #333;
        font-size: 16px;
        line-height: 1.5;
        font-family: $font-hei;
        background: #f5f5f5;
    }
</style>

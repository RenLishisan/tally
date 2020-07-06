<template>
    <div class="nav-wrapper">
        <div class="content">
            <slot/>
        </div>
        <Nav/>
    </div>
</template>
<script lang="ts">
    export default {
        name: 'Layout'
    };
</script>
<style lang="scss" scoped>
    .nav-wrapper {
        border: 1px solid red;
        display: flex;
        flex-direction: column;
        min-height: 100vh;
    }
    .content {
        border: 1px solid blue;
        flex-grow: 1;
        overflow: auto;
    }
    svg{
        color: #2c3e50;
    }
</style>
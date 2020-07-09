<template>
    <div>
        <label class="notes">
            {{value}}
            <span class="name">备注</span>
            <input :value='value' @input="onInput"
                   placeholder="点击输入备注！"
                   type="text">
        </label>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';

    @Component
    export default class Notes extends Vue {
        value = '';

        onInput(event: KeyboardEvent) {
            const input = event.target as HTMLInputElement;
            this.value = input.value;
        }
    }
</script>

<style lang="scss" scoped>
    .notes {
        font-size: 14px;
        background: #f5f5f5;
        padding-left: 16px;
        display: flex;
        align-items: center;

        .name {
            padding-right: 16px;
        }

        input {
            height: 64px;
            flex-grow: 1;
            background: transparent;
            border: none;
            padding-right: 16px;
        }
    }

</style>
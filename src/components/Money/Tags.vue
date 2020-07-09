<template>
    <div class="tags">
        <div class="new">
            <button>新增加标签</button>
        </div>
        <ul class="current">
            <li :class="{selected: selectedTags.indexOf(tag)>=0}" :key="tag"
                @click="toggle(tag)"
                v-for="tag in dataSource">{{tag}}
            </li>
        </ul>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component, Prop} from 'vue-property-decorator';

    @Component
    export default class Tags extends Vue {
        @Prop(Array) dataSource: string[] | undefined;
        selectedTags: string[] = [];

        toggle(tag: string) {
            const index = this.selectedTags.indexOf(tag);
            if (index >= 0) {
                this.selectedTags.splice(index,  1);
            } else {
                this.selectedTags.push(tag);
            }
        }
    }
</script>

<style lang="scss" scoped>
    .tags {
        font-size: 14px;
        padding: 16px;
        flex-grow: 1;
        display: flex;
        flex-direction: column-reverse;

        > .current {
            display: flex;
            flex-wrap: wrap;

            > li {
                $bg: #d9d9d9;
                background: $bg;
                $h: 24px;
                height: $h;
                line-height: $h;
                border-radius: $h/2;
                padding: 0 16px;
                margin-right: 12px;
                margin-top: 4px;

                &.selected {
                    background: darken($bg, 50%);
                    color: white;
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
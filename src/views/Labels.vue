<template>
    <layout>
        <div class="tags">
            <router-link :key="tag.id" :to="`/labels/edit/${tag.id}`" class="tag"
            v-for="tag in tags">
                <span>{{tag.name}}</span>
                <Icon name="right"/>
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
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import tagListModel from '@/models/tagListModel';
    import Button from '@/components/Button.vue';

    @Component({
        components: {Button}
    })
    export default class Labels extends Vue {
        tags = window.tagList;


        createTag() {
            const name = window.prompt('请输入标签名');
            if (name) {
                const message = tagListModel.create(name);
                if (message === 'duplicated') {
                    window.alert('这个标签重复啦！');
                }else if(message === 'success'){
                    window.alert('标签添加成功！');
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
    .tags {
        background: white;
        font-size: 16px;
        padding-left: 16px;

        > .tag {
            min-height: 44px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid #e6e6e6;

            svg {
                width: 18px;
                height: 18px;
                color: #87CEFA;
                margin-right: 16px;
            }
        }
    }

    .createTag {
        background: #87CEFA;
        color: white;
        border-radius: 4px;
        border: none;
        height: 40px;
        padding: 0 16px;

        &-wrapper {
            text-align: center;
            padding: 16px;
            margin-top: 44-16px;
        }
    }
</style>
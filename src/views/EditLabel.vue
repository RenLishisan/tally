<template>
    <Layout>
        <div class="navBar">
            <Icon @click="goBack" class="leftIcon" name="left"/>
            <span class="title">编辑标签</span>
            <span class="rightIcon"/>
        </div>
        <div class="form-wrapper">
            <FormItem :value="currentTag.name"
                      @update:value="update"
                      field-name="标签名" placeholder="请输入标签名"/>
        </div>
        <div class="button-wrapper">
            <Button @click="remove">删除标签</Button>
        </div>
    </Layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import FormItem from '@/components/Money/Formltem.vue';
    import Button from '@/components/Button.vue';

    @Component({
        components: {Button, FormItem}
    })
    export default class EditLabel extends Vue {
        get currentTag() {
            return this.$store.state.currentTag;
        }

        created() {
            const id = this.$route.params.id;
            this.$store.commit('fetchTags');
            this.$store.commit('setCurrentTag', id);
            if (!this.currentTag) {
                this.$router.replace('/404');
            }
        }

        update(name: string) {
            if (this.currentTag) {
                this.$store.commit('updateTag', {id: this.currentTag.id, name});
            }
        }

        remove() {
            if(this.currentTag){
                this.$store.commit('removeTag',this.currentTag.id)
            }
        }

        goBack() {
            this.$router.back();
        }
    }
</script>

<style lang="scss" scoped>
    .navBar {
        text-align: center;
        font-size: 16px;
        padding: 12px 16px;
        display: flex;
        align-items: center;
        justify-content: space-between;
      border-radius:0 0 12px 12px;
      background: #d5e3ec;
      box-shadow:  8px 8px 16px #c0ccd4,
      -8px -8px 16px #eafaff;

        > .title {

        }

        .leftIcon {
            width: 24px;
            height: 24px;
        }

        .rightIcon {
            width: 24px;
            height: 24px;
        }
    }

    .form-wrapper {
        margin-top: 8px;
      border-radius: 8px;
      background: #d5e3ec;
      box-shadow: inset 8px 8px 16px #c0ccd4,
      inset -8px -8px 16px #eafaff;
    }

    .button-wrapper {
        text-align: center;
        padding: 16px;
        margin-top: 44-16px;
    }
</style>
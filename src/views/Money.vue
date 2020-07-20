<template>
    <layout class-prefix="layout">
        <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
        <Types :value.sync="record.type"/>
        <div class="notes">
            <FormItem @update:value="onUpdateNotes"
                      field-name="备注"
                      placeholder="记得在这里输入备注嗷！"/>
        </div>
        <Tags :data-source.sync="tags" @update:value="onUpdateTags"/>
    </layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import NumberPad from '@/components/Money/NumberPad.vue';
    import Types from '@/components/Money/Types.vue';
    import FormItem from '@/components/Money/Formltem.vue';
    import Tags from '@/components/Money/Tags.vue';
    import {Component} from 'vue-property-decorator';
    import store from '@/store/index2.ts';

    @Component({
        components: {Tags, FormItem, Types, NumberPad},
    })
    export default class Money extends Vue {
        tags = store.tagList;
        recordList = store.recordList;
        record: RecordItem = {
            tags: [], notes: '', type: '-', amount: 0
        };

        onUpdateTags(value: string[]) {
            this.record.tags = value;
            this.recordList.push(this.record);
        }

        onUpdateNotes(value: string) {
            this.record.notes = value;
        }

        saveRecord() {
            store.createRecord(this.record);
        }

    }
</script>
<style lang="scss">
    .layout-content {
        display: flex;
        flex-direction: column-reverse;
    }

    .notes {
        padding: 12px 0;
    }
</style>


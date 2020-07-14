<template>
    <layout class-prefix="layout">
        <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
        <Types :value.sync="record.type"/>
        <Notes @update:value="onUpdateNotes"/>
        <Tags :data-source.sync="tags" @update:value="onUpdateTags"/>
    </layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import NumberPad from '@/components/Money/NumberPad.vue';
    import Types from '@/components/Money/Types.vue';
    import Notes from '@/components/Money/Notes.vue';
    import Tags from '@/components/Money/Tags.vue';
    import {Component, Watch} from 'vue-property-decorator';
    import model from '@/model';


    const version = window.localStorage.getItem('version') || '0';
    const recordList = model.fetch();

    if (version === '0.0.1') {
        // 数据升级&数据迁移
        recordList.forEach(record => {
            record.createdAt = new Date(2020, 0, 1);
        });
        // 保存数据
        window.localStorage.setItem('recordList', JSON.stringify(recordList));
    }
    window.localStorage.setItem('version', '0.0.2');



    @Component({
        components: {Tags, Notes, Types, NumberPad},
    })
    export default class Money extends Vue {
        tags = ['衣', '食', '住', '行', '意外'];
        recordList: RecordItem[] = JSON.parse(window.localStorage.getItem('recordList') || '[]');
        record: RecordItem = {
            tags: [], notes: '', type: '-', amount: 0
        };

        onUpdateTags(value: string[]) {
            this.record.tags = value;
            this.recordList.push(this.record);
        }

        onUpdateNotes(value: string) {
            this.record.notes = value;
            console.log(this.recordList);
        }

        saveRecord() {
            const record2: RecordItem =model.clone(this.record);
            record2.createdAt = new Date();
            this.recordList.push(record2);
        }

        @Watch('recordList')
        onRecordListChange() {
           model.save(this.recordList);
        }
    }
</script>
<style lang="scss">
    .layout-content {
        display: flex;
        flex-direction: column-reverse;
    }
</style>


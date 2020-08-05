<template>
  <layout class-prefix="layout">
    <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
    <Tabs :data-source="recordTypeList"
          :value.sync="record.type"/>
    <div class="notes">
      <FormItem :value.sync="record.notes"
                field-name="备注："
                placeholder="记得在这里输入备注嗷！"/>
    </div>

    <Tags @update:value="record.tags =$event"/>
    <div class="dates">
      <FormItem :value.sync="record.createdAt"
                field-name="日期选择："
                placeholder="记得选择日期呀！"
                type="date"/>
    </div>
  </layout>
</template>

<script lang="ts">
  import Vue from 'vue';
  import NumberPad from '@/components/Money/NumberPad.vue';
  import FormItem from '@/components/Money/Formltem.vue';
  import Tags from '@/components/Money/Tags.vue';
  import {Component} from 'vue-property-decorator';
  import Tabs from '@/components/Tabs.vue';
  import recordTypeList from '@/constants/recordTypeList';

  @Component({
    components: {Tabs, Tags, FormItem, NumberPad},
  })
  export default class Money extends Vue {
    recordTypeList = recordTypeList;
    record: RecordItem = {
      tags: [], notes: '', type: '-', amount: 0,createdAt:new Date().toISOString()
    };

    get recordList() {
      return this.$store.state.recordList;
    }

    created() {
      this.$store.commit('fetchRecords');
    }

    onUpdateNotes(value: string) {
      this.record.notes = value;
    }

    saveRecord() {
      if (!this.record.tags || this.record.tags.length == 0) {
        return window.alert('最少也得选择一个标签嘛！');
      }
      this.$store.commit('createRecord', this.record);
      if (this.$store.state.createRecordError === null) {
        window.alert('账单记录成功！');
        this.record.notes = '';
      }

    }
  }
</script>
<style lang="scss" scoped>
  ::v-deep .layout-content {
    display: flex;
    flex-direction: column-reverse;
  }

  .notes {
    padding: 10px 0;
    z-index: 1;
    border-radius: 12px;
    background: #d5e3ec;
    box-shadow: inset 4px 4px 10px #a8b3ba, inset -1px -2px 8px #FFFFFF;
    position: relative;
    top: 6px;
  }
  .dates {
    padding:1px 0;
    z-index: 1;
    border-radius:0 0  12px 12px;
    background: #d5e3ec;
    box-shadow:  -25px 4px 8px #c0ccd4,
    4px -4px 8px #eafaff;
    top: 6px;
  }

</style>


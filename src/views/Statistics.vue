<template>
  <Layout>
    <Tabs :data-source="recordTypeList"
          :value.sync="type"/>
<!--    <div class="chart-wrapper" ref="chartWrapper">-->
<!--      <ECharts :options="chartOptions" class="chart"/>-->
<!--    </div>-->
    <ol class="left" v-if="groupedList.length>0">
      <li :key="index" v-for="(group,index) in groupedList">
        <h3 class="title">{{ beautify(group.title) }} <span>￥{{ group.total }}</span></h3>
        <ol>
          <li :key="item.id" class="record"
              v-for="item in group.items">
            <span>{{ tagString(item.tags) }}</span>
            <span class="notes">{{ item.notes }}</span>
            <span>￥{{ item.amount }}</span>
          </li>
        </ol>
      </li>
    </ol>
    <div class="noResult" v-else>
      主人当前没有进行记账哦！
    </div>
  </Layout>
</template>


<script lang="ts">
  import Vue from 'vue';
  import {Component} from 'vue-property-decorator';
  import Tabs from '@/components/Tabs.vue';
  import recordTypeList from '@/constants/recordTypeList';
  import dayjs from 'dayjs';
  import clone from '@/lib/clone';

  const ECharts: any = require('vue-echarts').default;
  import 'echarts/lib/chart/line';
  import 'echarts/lib/component/polar';
  import _ from 'lodash';
  import day from 'dayjs';


  @Component({
    components: {Tabs, ECharts}
  })
  export default class Statistics extends Vue {

    get groupedList() {
      const {recordList} = this;
      const newList = clone(recordList)
          .filter(r => r.type === this.type)
          .sort((a, b) => dayjs(b.createdAt).valueOf() - dayjs(a.createdAt).valueOf());
      if (newList.length === 0) {return [];}
      type Result = { title: string; total?: number; items: RecordItem[] }[]
      const result: Result = [{title: dayjs(newList[0].createdAt).format('YYYY-MM-DD'), items: [newList[0]]}];
      for (let i = 1; i < newList.length; i++) {
        const current = newList[i];
        const last = result[result.length - 1];
        if (dayjs(last.title).isSame(dayjs(current.createdAt), 'day')) {
          last.items.push(current);
        } else {
          result.push({title: dayjs(current.createdAt).format('YYYY-MM-DD'), items: [current]});
        }
      }
      result.map(group => {
        group.total = group.items.reduce((sum, item) => sum + item.amount, 0);
      });
      return result;
    }

    get chartOptions() {
      const today = new Date();
      const array = [];
      for (let i = 0; i <= 29; i++) {
        const dateString = day(today)
            .subtract(i, 'day').format('YYYY-MM-DD');
        const found = _.find(this.recordList, {
          createdAt: dateString
          });
        array.push({
          date: dateString, value: found ? found.amount : 0
        });
      }
      const keys = array.map(item => item.date);
      const values = array.map(item => item.value)
      return {
        itemStyle: {color: 'rgb(176,213,223)'},
        lineStyle: {color: 'rgb(34,162,195)'},

        grid: {
          left: 0,
          right: 0,
          top: 0

        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: ['1号', '2号', '3号', '4号', '5号', '6号', '7号', '8号', '9号', '10号', '11号', '12号', '13号', '14号', '15号', '16号', '17号', '18号', '19号', '20号', '21号', '22号', '23号', '24号', '25号', '26号', '27号', '28号', '29号', '30号']
        },
        yAxis: {
          type: 'value',
          show: false
        },
        series: [{
          data: values,
          type: 'line',
          areaStyle: {}
        }]
      };
    }

    tagString(tags: Tag[]) {
      return tags.length === 0 ? '未添加标签' : tags.map(t => t.name).join(',');
    }


    get recordList() {
      return (this.$store.state as RootState).recordList;
    }

    mounted() {
      const div = (this.$refs.chartWrapper as HTMLDivElement);
      div.scrollLeft = div.scrollWidth;
    }

    beautify(string: string) {
      const day = dayjs(string);
      const now = dayjs();
      if (day.isSame(now, 'day')) {
        return '今天';
      } else if (day.isSame(now.subtract(1, 'day'), 'day')) {
        return '昨天';
      } else if (day.isSame(now.subtract(2, 'day'), 'day')) {
        return '前天';
      } else if (day.isSame(now, 'year')) {
        return day.format('MM月D日');
      } else {
        return day.format('YYYY年MM月D日');
      }
    }

    beforeCreate() {
      this.$store.commit('fetchRecords');
    }

    type = '-';
    recordTypeList = recordTypeList;

  }
</script>

<style lang="scss" scoped>
  .chart {
    width: 400%;

    &-wrapper {
      overflow: auto;

      &::-webkit-scrollbar {
        display: none;
      }
    }
  }


  .noResult {
    border-radius: 13px;
    background: #d5e3ec;
    box-shadow:  6px 6px 11px #b5c1c9,
    -6px -6px 11px #f5ffff;
    padding: 16px;
    text-align: center;
    margin: 50px;
  }

  %item {
    padding: 8px 16px;
    line-height: 24px;
    display: flex;
    justify-content: space-between;
    align-content: center;
  }

  .notes {
    margin-right: auto;
    margin-left: 16px;
    color: #999;
  }

  .title {
    border-radius: 8px;
    background: #d5e3ec;
    box-shadow:  6px 6px 13px #b5c1c9,
    -6px -6px 13px #f5ffff;
    @extend %item;
  }

  .record {
    @extend %item
  }

  ::v-deep {
    .type-tabs-item {
      background: #c4c4c4;

      &.selected {
        background: white;

        &::after {
          display: none;
        }
      }
    }

    .interval-tabs-item {
      height: 48px;
    }
  }
  .left{
    margin: 10px;
  }


</style>
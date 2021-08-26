<template>
  <v-sheet
    width="100%"
    rounded
    elevation="2"
    class="pt-3 py-2 pb-2 covid-chart"
    :color="
      darkMode ? theme.dark.headerBackground : theme.light.headerBackground
    "
  >
    <Loading v-if="trendingsLineVietnamCases.loading" />
    <div class="text-h6 font-weight-bold text-center" v-text="label" />
    <apexchart
      class="mb-4"
      width="100%"
      type="area"
      :options="{
        ...options,
        xaxis: {
          categories: data.dates,
          tickPlacement: 'on',
          tickAmount: 3,
          labels: {
            rotate: 0,
            rotateAlways: false,
          },
        },
      }"
      :series="[
        { name: 'Số ca nhiễm', data: data.confirmed },
        { name: 'Tử vong', data: data.deaths },
        { name: 'Hồi phục', data: data.recovered },
      ]"
    />
    <v-row class="mb-2 px-4">
      <v-col
        v-for="(range, index) in ranges"
        :key="index"
        md="3"
        cols="6"
        class="d-flex justify-center"
      >
        <v-btn
          width="100%"
          dark
          light
          :outlined="selectedRange.value !== range.value"
          :color="theme.default.danger"
          @click="selectRange(index)"
          >{{ range.name }}</v-btn
        >
      </v-col>
    </v-row>
  </v-sheet>
</template>

<script lang="ts">
import Vue from 'vue';
import Loading from '@/components/common/Loading.vue';
import { TrendingLineCasesData } from '@/store/home';
import { theme } from '@/themes';

interface Range {
  name: string;
  value: number;
}

const ranges: Range[] = [
  { name: '1 tháng', value: 30 },
  { name: '2 tháng', value: 60 },
  { name: '3 tháng', value: 90 },
  { name: '6 tháng', value: 180 },
];

export default Vue.extend({
  components: {
    Loading,
  },
  props: {
    type: {
      type: String,
      default: 'area',
    },
  },
  data() {
    return {
      label: 'Diễn biến dịch tại Việt Nam',
      ranges,
      selectedRange: ranges[0],
    };
  },
  computed: {
    darkMode() {
      return this.$store.state.common.darkMode;
    },
    trendingsLineVietnamCases() {
      return this.$store.state.home.trendingLineCases;
    },
    data() {
      const trendingsLineVietnamCases =
        this.$store.state.home.trendingLineCases.data;
      const dates: string[] = [];
      const confirmed: number[] = [];
      const recovered: number[] = [];
      const deaths: number[] = [];
      trendingsLineVietnamCases.forEach((item: TrendingLineCasesData) => {
        dates.push(item.date);
        confirmed.push(item.confirmed);
        recovered.push(item.recovered);
        deaths.push(item.deaths);
      });
      return {
        dates: dates.slice(
          Math.max(dates.length - this.$data.selectedRange.value, 1)
        ),
        confirmed: confirmed.slice(
          Math.max(confirmed.length - this.$data.selectedRange.value, 1)
        ),
        recovered: recovered.slice(
          Math.max(recovered.length - this.$data.selectedRange.value, 1)
        ),
        deaths: deaths.slice(
          Math.max(deaths.length - this.$data.selectedRange.value, 1)
        ),
      };
    },
    options() {
      return {
        theme: {
          mode: (this.$store as any).state.common.darkMode ? 'dark' : 'light',
        },
        dataLabels: {
          enabled: false,
        },
        colors: [
          theme.default.warning,
          theme.default.danger,
          theme.default.success,
        ],
        chart: {
          id: 'chart',
          zoom: false,
          toolbar: {
            show: false,
          },
        },
      };
    },
    theme() {
      return theme;
    },
  },
  methods: {
    selectRange(index: number) {
      this.$data.selectedRange = ranges[index];
    },
  },
});
</script>
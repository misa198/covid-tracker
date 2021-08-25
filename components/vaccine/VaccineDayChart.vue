<template>
  <v-sheet
    width="100%"
    rounded
    elevation="2"
    class="pt-3 py-2 vaccine-chart"
    :color="
      darkMode ? theme.dark.headerBackground : theme.light.headerBackground
    "
  >
    <div
      class="text-h6 font-weight-bold text-center"
      v-text="'Mũi tiêm theo ngày'"
    />
    <apexchart width="100%" :options="data.options" :series="data.series" />
  </v-sheet>
</template>

<script lang="ts">
import Vue from 'vue';
import { AppVietnamVaccineStatistic } from '~/services/app.service';
import { theme } from '~/themes';

export default Vue.extend({
  computed: {
    data() {
      const statisticData = this.$store.state.vaccine.statistic
        .data as AppVietnamVaccineStatistic;
      const selected = [...statisticData.dayInjection];
      const dates: string[] = [];
      const first: number[] = [];
      const second: number[] = [];
      const average: number[] = [];

      selected.forEach((item) => {
        dates.push(item.date);
        first.push(item.first);
        second.push(item.second);
        average.push(item.average);
      });
      const sm = (this as any).$vuetify.breakpoint.smAndDown;

      return {
        options: {
          theme: {
            mode: (this.$store as any).state.common.darkMode ? 'dark' : 'light',
          },
          dataLabels: {
            enabled: false,
          },
          colors: [
            theme.default.success,
            theme.default.indigo,
            theme.default.lime,
          ],
          chart: {
            id: 'chart',
            zoom: false,
            toolbar: {
              show: false,
            },
          },
          xaxis: {
            categories: dates,
            tickPlacement: 'on',
            tickAmount: sm ? 2 : 3,
            labels: {
              rotate: 0,
              rotateAlways: false,
            },
          },
        },
        series: [
          { name: 'Tiêm mũi 1', data: first, type: 'area' },
          { name: 'Tiêm mũi 2', data: second, type: 'area' },
          { name: 'Trung bình 7 ngày', data: average, type: 'line' },
        ],
      };
    },
    darkMode() {
      return (this.$store as any).state.common.darkMode;
    },
    theme() {
      return theme;
    },
  },
});
</script>
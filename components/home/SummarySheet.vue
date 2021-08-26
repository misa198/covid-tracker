<template>
  <HomeContainer>
    <v-sheet
      class="ma-auto mt-1 py-3 px-3"
      elevation="2"
      rounded
      :color="
        darkMode ? theme.dark.headerBackground : theme.light.headerBackground
      "
      :style="{ position: 'relative' }"
    >
      <Loading v-if="summary.loading" />
      <v-sheet
        class="mx-auto"
        :color="
          darkMode
            ? theme.dark.secondaryBackground
            : theme.light.secondaryBackground
        "
        width="100%"
        rounded
      >
        <div
          class="text-h6 font-weight-bold text-center py-2"
          v-text="headerTitle"
        />
      </v-sheet>
      <div
        class="subtitle-1 font-weight-bold text-center pt-4 pb-2 red--text"
        v-text="bodyTitle"
      />
      <div
        class="ma-auto px-6 pb-2"
        :style="{
          flexDirection: 'column !important',
          width: '100%',
        }"
      >
        <v-row>
          <SumaryItem
            :show-daily-value="true"
            :color="theme.default.warning"
            :sub-color="theme.default.warningSecondary"
            :title="'Ca nhiễm'"
            :total-value="summary.data.confirmed"
            :daily-value="summary.data.confirmed - summary.data.lastConfirmed"
            :loading="summary.loading"
          />
          <SumaryItem
            :show-daily-value="true"
            :color="theme.default.success"
            :sub-color="theme.default.successSecondary"
            :title="'Hồi phục'"
            :total-value="summary.data.recovered"
            :daily-value="summary.data.recovered - summary.data.lastRecovered"
            :loading="summary.loading"
          />
        </v-row>
        <v-row>
          <SumaryItem
            :show-daily-value="true"
            :color="theme.default.danger"
            :sub-color="theme.default.dangerSecondary"
            :title="'Tử vong'"
            :total-value="summary.data.deaths"
            :daily-value="summary.data.deaths - summary.data.lastDeaths"
            :loading="summary.loading"
          />
          <SumaryItem
            :show-daily-value="false"
            :color="theme.default.info"
            :sub-color="theme.default.infoSecondary"
            :title="'Đang điều trị'"
            :total-value="summary.data.curing"
            :loading="summary.loading"
          />
        </v-row>
      </div>
    </v-sheet>
  </HomeContainer>
</template>
<script lang="ts">
import Vue from 'vue';
import HomeContainer from '@/components/common/HomeContainer.vue';
import SumaryItem from '@/components/home/SummaryItem.vue';
import Loading from '@/components/common/Loading.vue';
import { theme } from '@/themes';

export default Vue.extend({
  components: {
    SumaryItem,
    HomeContainer,
    Loading,
  },
  data() {
    return {
      headerTitle: 'Số liệu Covid-19 tại Việt Nam',
      bodyTitle: 'Kể từ khi dịch bùng phát từ đầu 2020 đến nay',
    };
  },
  computed: {
    darkMode() {
      return this.$store.state.common.darkMode;
    },
    theme() {
      return theme;
    },
    summary() {
      return this.$store.state.home.summary;
    },
  },
});
</script>
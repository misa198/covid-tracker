<template>
  <v-col cols="6" class="px-2">
    <div
      v-if="showSubtitle"
      class="subtitle-2 text-center"
      :style="{ color: darkMode && isCuring ? '#e5e7eb' : color }"
      v-text="
        `Hôm nay: ${
          (dailyValue < 0 ? '-' : '+') +
          formatNumberMethod(Math.abs(dailyValue))
        }`
      "
    />
    <div
      class="text-h5 font-weight-black text-center"
      :style="{ color: darkMode && isCuring ? '#e5e7eb' : color }"
      v-text="formatNumberMethod(totalValue)"
    />

    <v-sheet class="mx-auto mt-2" :color="subColor" width="100%" rounded>
      <div
        class="subtitle-1 font-weight-bold text-center"
        :style="{ color: color }"
        v-text="title"
      />
    </v-sheet>
  </v-col>
</template>
<script lang="ts">
import Vue from 'vue';
import { formatNumber } from '@/utils/formatNumber';

export default Vue.extend({
  props: {
    color: {
      type: String,
      default: 'primary',
    },
    subColor: {
      type: String,
      default: 'secondary',
    },
    title: {
      type: String,
      default: '',
    },
    dailyValue: {
      type: Number,
      default: 0,
    },
    totalValue: {
      type: Number,
      default: 0,
    },
    isCuring: {
      type: Boolean,
      default: false,
    },
    showSubtitle: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    darkMode() {
      return this.$store.state.common.darkMode;
    },
  },
  methods: {
    formatNumberMethod(value: number) {
      return formatNumber(value);
    },
  },
});
</script>

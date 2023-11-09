<script setup lang="ts">

import FlashBronze from "../../assets/icons/rank/flash-bronze.vue";
import FlashSilver from "../../assets/icons/rank/flash-silver.vue";
import FlashGold from "../../assets/icons/rank/flash-gold.vue";
import FlashDiamond from "../../assets/icons/rank/flash-diamond.vue";
import FlashEmerald from "../../assets/icons/rank/flash-emerald.vue";

import RankCard from "./components/RankCard.vue";

const dataRank = [
  {
    rank: "bronze",
    icon: FlashBronze,
    info: [
      {
        title: "7 day deposit req.",
        value: "0%",
      },
      {
        title: "Discount",
        value: "0 USD",
      },
    ],
  },
  {
    rank: "silver",
    icon: FlashSilver,
    info: [
      {
        title: "7 day deposit req.",
        value: "5%",
      },
      {
        title: "Discount",
        value: "700 USD",
      },
    ],
  },
  {
    rank: "gold",
    icon: FlashGold,
    info: [
      {
        title: "7 day deposit req.",
        value: "10%",
      },
      {
        title: "Discount",
        value: "1 323 USD",
      },
    ],
  },
  {
    active: true,
    rank: "diamond",
    icon: FlashDiamond,
    info: [
      {
        title: "7 day deposit req.",
        value: "15%",
      },
      {
        title: "Discount",
        value: "2 489 USD",
      },
    ],
  },
  {
    rank: "emerald",
    icon: FlashEmerald,
    info: [
      {
        title: "7 day deposit req.",
        value: "20%",
      },
      {
        title: "Discount",
        value: "5 445 USD",
      },
    ],
  },
];

defineProps<{ theme?: "dark" | "light"; }>()

</script>

<template>
  <div :class="['Rank', theme]">
    <RankCard v-for="(e, i) in dataRank" :theme="theme" :key="i" v-bind="e" />
  </div>
</template>

<style scope lang="scss">
.Rank {
  display: flex;
  justify-content: center;
  gap: 20px;

  width: fit-content;
  padding: 40px;

  background-color: #fff;
  border-radius: 20px;

  &.dark {
    background-color: #0d0d0d;
  }
}
</style>

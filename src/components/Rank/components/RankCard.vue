<script setup lang="ts">
import gsap from 'gsap';
import { DefineComponent, onMounted, ref } from 'vue';

import blub from '../../../assets/blub.vue';

const refBlob = ref<HTMLDivElement>()
const refBlobBack = ref<HTMLDivElement>()

const props = defineProps<{
  theme?: "dark" | "light";
  active?: boolean;
  rank: string;
  icon: DefineComponent;
  info: {
    title: string;
    value: string;
  }[];
}>()

onMounted(() => {
  const theme = props.theme;
  console.log(theme)

  let ctx = gsap.context(() => {
    let tl1 = gsap.timeline({
      defaults: {
        delay: 0,
        duration: 1.6,
        ease: "power1.inOut",
        repeat: -1,
        yoyo: true,
      },
    });

    let tl2 = gsap.timeline({
      defaults: {
        delay: 1,
        duration: 3,
        ease: "power1.inOut",
        repeat: -1,
        yoyo: true,
      },
    });

    if (refBlob.value && refBlobBack.value) {
      tl1.fromTo(
        refBlob.value,
        {
          scale: 1,
          opacity: 0.5,
        },
        {
          scale: theme == "light" ? 1.2 : 1.1,
          opacity: theme == "light" ? 0.6 : 0.7,
        },
        "<10%"
      );

      tl2.fromTo(
        refBlobBack.value,
        {
          scale: 0.6,
          opacity: 0,
        },
        { scale: "light" ? 1.2 : 1.1, opacity: theme == "light" ? 0.4 : 0.4 }
      );
    }
  });

  return () => ctx.revert();
})

</script>

<template>
  <div :class="['RankCard', theme, active && 'active']">
    <div class="RankCard_blubBox">
      <div class="RankCard_blub" ref="refBlob">
        <component :is="blub" />
      </div>
      <div :class="['RankCard_blub', 'back']" ref="refBlobBack">
        <component :is="blub" />
      </div>
    </div>
    <div :class="['RankCard_inner', rank, theme, active && 'active']">
      <div class="RankCard_icon">
        <component :is="icon" />
      </div>
      <div class="RankCard_title">{{ rank }}</div>
      <div class="RankCard_info">
        <div v-for="(e, i) in info" class="RankCard_info_item" :key="i">
          <div class="RankCard_info_item_title">
            {{ e.title }}
          </div>
          <div class="RankCard_info_item_value">
            {{ e.value }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scope lang="scss">
.RankCard {
  position: relative;

  z-index: 1;

  &.active {
    .RankCard_blubBox {
      display: block;
    }
  }

  &.light {
    .RankCard_blub {
      filter: blur(15px);
    }
  }
}

.RankCard_blubBox {
  display: none;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  z-index: 5;
}

.RankCard_blub {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  filter: blur(20px);

  // opacity: 0;

  z-index: 5;

  transform: scale(0.6);

  & svg {
    width: 100%;
    height: 100%;
  }

  &.back {
    transform: rotate(180deg);
    // transform-origin: top center;
    // opacity: 0 !important;

    filter: blur(10px);

    & path {
      fill: rgb(85, 159, 255);
    }
  }
}

.RankCard_inner {
  position: relative;
  padding: 12px;
  overflow: hidden;

  border-radius: 24px;
  opacity: 0.3;

  z-index: 20;

  &.light {
    &.bronze {
      background-color: #f6f6f6;
    }

    &.silver {
      background: linear-gradient(180deg,
          rgba(81, 182, 255, 0) 28.13%,
          rgba(81, 182, 255, 0.02) 79.69%,
          rgba(81, 182, 255, 0.08) 100%),
        #f6f6f6;
    }

    &.gold {
      background: linear-gradient(180deg,
          rgba(255, 238, 81, 0) 28.13%,
          rgba(255, 238, 81, 0.02) 79.69%,
          rgba(255, 238, 81, 0.08) 100%),
        #f6f6f6;
    }

    &.diamond {
      background: linear-gradient(180deg,
          rgba(81, 151, 255, 0) 28.13%,
          rgba(81, 151, 255, 0.07) 79.69%,
          rgba(81, 151, 255, 0.21) 100%),
        #f6f6f6;
    }

    &.emerald {
      background: linear-gradient(180deg,
          rgba(188, 255, 81, 0) 28.13%,
          rgba(188, 255, 81, 0.07) 79.69%,
          rgba(188, 255, 81, 0.21) 100%),
        #f6f6f6;
    }
  }

  &.dark {
    &.bronze {
      background-color: #1b1b1b;
    }

    &.silver {
      background: linear-gradient(180deg,
          rgba(81, 182, 255, 0) 28.13%,
          rgba(81, 182, 255, 0.02) 79.69%,
          rgba(81, 182, 255, 0.08) 100%),
        #1b1b1b;
    }

    &.gold {
      background: linear-gradient(180deg,
          rgba(255, 238, 81, 0) 28.13%,
          rgba(255, 238, 81, 0.02) 79.69%,
          rgba(255, 238, 81, 0.08) 100%),
        #1b1b1b;
    }

    &.diamond {
      background: linear-gradient(180deg,
          rgba(81, 151, 255, 0) 28.13%,
          rgba(81, 151, 255, 0.07) 79.69%,
          rgba(81, 151, 255, 0.21) 100%),
        #1b1b1b;
    }

    &.emerald {
      background: linear-gradient(180deg,
          rgba(188, 255, 81, 0) 28.13%,
          rgba(188, 255, 81, 0.07) 79.69%,
          rgba(188, 255, 81, 0.21) 100%),
        #1b1b1b;
    }

    .RankCard_title {
      color: #fff;
    }

    .RankCard_info_item {
      background: rgba(175, 175, 175, 0.08);
    }

    .RankCard_info_item_title {
      color: rgba(255, 255, 255, 0.5);
    }

    .RankCard_info_item_value {
      color: #fff;
    }
  }

  &.active {
    opacity: 1;

    //   &.bronze {
    //     box-shadow: 0px 0px 70px 0px #dbb172a5;
    //   }

    //   &.silver {
    //     box-shadow: 0px 0px 70px 0px #6dccff88;
    //   }

    //   &.gold {
    //     box-shadow: 0px 0px 70px 0px rgba(255, 218, 11, 0.516);
    //   }

    //   &.diamond {
    //     box-shadow: 0px 0px 70px 0px #60aaff94;
    //   }

    //   &.emerald {
    //     box-shadow: 0px 0px 70px 0px #bdff5278;
    //   }
  }
}

.RankCard_icon {
  display: flex;
  justify-content: center;
  height: 100px;

  & svg {
    height: 100%;
    width: auto;
  }
}

.RankCard_title {
  margin-top: 16px;

  color: #000;
  text-align: center;
  font-size: 20px;
  font-weight: 600;
  line-height: 117.7%;
  text-transform: capitalize;
}

.RankCard_info {
  display: flex;
  flex-direction: column;
  gap: 5px;
  margin-top: 16px;
}

.RankCard_info_item {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 12px;

  border-radius: 12px;
  background: #fff;
}

.RankCard_info_item_title {
  color: rgba(0, 0, 0, 0.5);
  font-size: 14px;
  font-weight: 500;
  line-height: 117.7%;
}

.RankCard_info_item_value {
  color: #000;
  font-size: 16px;
  font-weight: 600;
  line-height: 117.7%;
}
</style>
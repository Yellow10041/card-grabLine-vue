<script setup lang="ts">

import { onMounted, ref } from "vue";

import Flash_dark from "../assets/flash-dark.png";
import Flash_light from "../assets/flash-light.png";

import gsap from "gsap";
import { Draggable } from "gsap/all";
gsap.registerPlugin(Draggable);

const refDrag = ref<HTMLDivElement>();
const tongueRef = ref<HTMLDivElement>();
const refPercent = ref<HTMLDivElement>();
const refArrow = ref<HTMLDivElement>();
const tongueBackRef = ref<HTMLDivElement>();
const refDiscount = ref<HTMLDivElement>();
const refGetIt = ref<HTMLButtonElement>();
const refImgFront = ref<HTMLDivElement>();
const refImgBack = ref<HTMLDivElement>();
const refGetItAnimBox = ref<HTMLDivElement>();
const refGetItAnim = ref<HTMLDivElement>();

const handleButton = (e: MouseEvent) => {
  if (refGetIt.value && refGetItAnim.value && refGetItAnimBox.value) {
    console.log(e);
    const { x: buttonX, y: buttonY } =
      refGetIt.value.getBoundingClientRect();
    const { clientX: mouseX, clientY: mouseY } = e;

    console.log(buttonX, buttonY);
    console.log(mouseX, mouseY);

    const animX = mouseX - buttonX;
    const animY = mouseY - buttonY;

    console.log(animX, animY);

    gsap.set(refGetItAnimBox.value, {
      x: animX,
      y: animY,
    });

    gsap.set(refGetItAnim.value, {
      opacity: 1,
    });

    gsap.fromTo(
      refGetItAnim.value,
      {
        width: 0,
        height: 0,
        scale: 0,
      },
      {
        width: refGetIt.value.offsetWidth * 3,
        height: refGetIt.value.offsetWidth * 3,
        scale: 1,
        duration: 0.5,
        ease: "power2.inOut",
        onComplete: () => {
          if (refGetItAnim.value) {
            gsap.set(refGetItAnim.value, {
              width: 0,
              height: 0,
              opacity: 0,
            });
          }
        },
      }
    );
  }
}

onMounted(() => {
  if (refImgFront.value && refImgBack.value && tongueRef.value && refDiscount.value && refDiscount.value) {


    const duration = 2;
    const deg = 0;
    const tl1 = gsap.timeline({
      defaults: {
        delay: 0,
        duration: duration,
        ease: "power2.inOut",
        repeat: -1,
        yoyo: true,
        yoyoEase: "power1.inOut",
      },
    });

    const tl2 = gsap.timeline({
      defaults: {
        delay: 0,
        duration: duration,
        ease: "power2.inOut",
        repeat: -1,
        yoyo: true,
        yoyoEase: "power1.inOut",
      },
    });

    tl1.fromTo(
      refImgFront.value,
      {
        yPercent: 0,
        rotate: 0,
      },
      {
        yPercent: -5,
        rotate: -deg,
      }
    );

    tl2.fromTo(
      refImgBack.value,
      {
        yPercent: 0,
        rotate: 0,
      },
      {
        yPercent: 10,
        rotate: -deg,
      }
    );

    const completeVal = 0.5;

    Draggable.create(tongueRef.value, {
      type: "x",
      bounds: refDrag.value,
      inertia: true,
      onDragStart: () => {
        refDiscount.value &&
          refDiscount.value.classList.add("active");
        refArrow.value && refArrow.value.classList.add("show");

        if (refDiscount.value) (
          gsap.to(refDiscount.value, {
            opacity: 0,
            duration: 0.2,
          })
        )

      },

      onDrag: function () {
        if (refDrag.value && tongueRef.value) {
          const progressCur =
            this.x /
            (refDrag.value.offsetWidth - tongueRef.value.offsetWidth);

          console.log(this);

          if (refDiscount.value && tongueBackRef.value) {
            gsap.set(tongueBackRef.value, {
              width: `${this.x + 40}px`,
            });

            gsap.set(refDiscount.value, {
              opacity: progressCur,
            });
          }

          if (progressCur >= completeVal) {
            console.log("complete");
          }
        }
      },

      onDragEnd: function () {
        if (refDrag.value && tongueRef.value) {
          const progressCur =
            this.x /
            (refDrag.value.offsetWidth - tongueRef.value.offsetWidth);

          if (refDiscount.value && tongueBackRef.value) {
            gsap.to(refDiscount.value, {
              opacity: 1,
              duration: 0.2,
            });
          }

          if (progressCur >= completeVal) {
            console.log("complete");

            gsap.to(tongueRef.value, {
              x: refDrag.value.offsetWidth - tongueRef.value.offsetWidth,
              zIndex: 100,
              duration: 0.4,
              ease: "power4.inOut",
            });

            if (refDiscount.value && tongueBackRef.value) {
              gsap.to(tongueBackRef.value, {
                width: `${refDrag.value.offsetWidth}px`,
                duration: 0.4,
                ease: "power4.inOut",
              });
            }
          } else {
            if (refDiscount.value && tongueBackRef.value) {
              gsap.to(tongueBackRef.value, {
                width: 40,
                duration: 0.4,
                ease: "power4.inOut",
              });

              gsap.to(refDiscount.value, {
                opacity: 0,
                duration: 0.2,
              });

              gsap.to(tongueRef.value, {
                x: 0,
                zIndex: 100,
                duration: 0.4,
                ease: "power4.inOut",
                onComplete: () => {
                  refDiscount.value &&
                    refDiscount.value.classList.remove("active");
                  refDiscount.value &&
                    refDiscount.value.classList.remove("active");

                  if (refDiscount.value && tongueBackRef.value) {
                    gsap.to(refDiscount.value, {
                      opacity: 1,
                      duration: 0.2,
                    });
                  }
                },
              });
            }

            console.log("end");

            refArrow.value &&
              refArrow.value.classList.remove("show");
          }
        }
      },
    })
  }
})

defineProps<{ theme: "dark" | "light"; }>()

</script>

<template>
  <div :class="['CardGrab', theme]">
    <div class="CardGrab_img">
      <div class="CardGrab_img_from" ref="refImgFront">
        <img :src="theme === 'dark' ? Flash_dark : Flash_light" alt="" />
      </div>

      <div class="CardGrab_img_back" ref="refImgBack">
        <img :src="theme === 'dark' ? Flash_dark : Flash_light" alt="" />

      </div>
    </div>

    <div class="CardGrab_drag">
      <div class="CardGrab_dragBox" ref="refDrag">
        <div class="CardGrab_tongueBack" ref="tongueBackRef" />
        <div class="CardGrab_percent" ref="refPercent">
          %
        </div>
        <div class="CardGrab_tongue" ref="tongueRef">
          <div class="CardGrab_arrow" ref="refArrow">
            <svg xmlns="http://www.w3.org/2000/svg" width="22" height="16" viewBox="0 0 22 16" fill="none">
              <path
                d="M21.7066 8.70711C22.0972 8.31658 22.0972 7.68342 21.7066 7.29289L15.3427 0.928932C14.9522 0.538408 14.319 0.538408 13.9285 0.928932C13.5379 1.31946 13.5379 1.95262 13.9285 2.34315L19.5853 8L13.9285 13.6569C13.5379 14.0474 13.5379 14.6805 13.9285 15.0711C14.319 15.4616 14.9522 15.4616 15.3427 15.0711L21.7066 8.70711ZM0.943359 9H20.9995V7H0.943359V9Z"
                fill="black" />
            </svg>
          </div>
        </div>
        <div class="CardGrab_discount" ref="refDiscount">
          Get discount up to 30%
        </div>
        <button class="CardGrab_getIt" ref="refGetIt" @click="handleButton">
          <div class="CardGrab_getIt_animBox" ref="refGetItAnimBox">
            <div class="CardGrab_getIt_anim" ref="refGetItAnim" />
          </div>
          Get it
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.CardGrab {
  max-width: 550px;
  padding: 25px;

  overflow: hidden;

  border-radius: 46px;
  background: linear-gradient(180deg,
      rgba(24, 24, 24, 0.43) 49.81%,
      rgba(37, 37, 37, 0.3) 70.24%),
    radial-gradient(67.61% 82.59% at 82.5% 72.85%,
      rgba(188, 255, 81, 0.26) 0%,
      rgba(188, 255, 81, 0) 100%),
    #161616;

  &.light {
    background: radial-gradient(83.31% 59.76% at 78.1% 86.38%,
        #f1fae7 0%,
        rgba(236, 250, 231, 0) 100%),
      #f6f6f6;

    .CardGrab_drag {
      background: #fff;
    }
  }
}

.CardGrab_img {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 425px;

  z-index: 10;

  & img {
    height: 100%;
    width: auto;

    &.bottom {
      position: absolute;
      top: 100%;
      transform: scaleY(-1);
      opacity: 0.3;
    }
  }
}

.CardGrab_img_back {
  position: absolute;
  top: 80%;
  left: 0;
  width: 100%;
  height: 100%;

  & img {
    transform: scaleY(-1);
    opacity: 0.3;
  }
}

.CardGrab_drag {
  position: relative;
  width: 100%;
  height: min-content;
  padding: 8px;

  border-radius: 100px;
  background: #0d0d0d;

  z-index: 20;
}

.CardGrab_dragBox {
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  width: 100%;
  height: 100%;
}

.CardGrab_tongueBack {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  min-width: 40px;
  border-radius: 46px;
  background: #bcff52;
  z-index: 20;
  // z-index: 100;
}

.CardGrab_tongue {
  position: relative;
  display: flex;
  align-items: center;
  width: 40px;
  height: 40px;
  border-radius: 46px;
  background: #bcff52;

  z-index: 20;
}

.CardGrab_percent {
  position: absolute;
  left: 0;
  top: 50%;
  transform: translate(0, -50%);

  width: 40px;
  text-align: center;

  color: #000;
  font-size: 16px;
  font-weight: 700;
  line-height: 117.7%;
  /* 18.832px */

  transition: 0.2s ease-in-out;
  transition-property: opacity;

  user-select: none;
  pointer-events: none;

  z-index: 1010;

  &.hidden {
    opacity: 0;
  }
}

.CardGrab_arrow {
  position: absolute;
  left: 0;
  top: 50%;
  transform: translate(0, -50%);

  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;

  opacity: 0;

  transition: 0.2s ease-in-out;
  transition-property: opacity;

  & svg {
    height: 50%;
    width: auto;
  }

  &.show {
    opacity: 1;
  }
}

.CardGrab_discount {
  color: #fff;
  font-size: 16px;
  font-weight: 600;
  line-height: 117.7%;
  /* 18.832px */

  transition: 0.2s ease-in-out;
  transition-property: color, text-shadow;

  mix-blend-mode: difference;

  z-index: 25;

  &.active {
    color: #000;
    text-shadow: -0px 2px 5px #fff;
  }
}

.CardGrab_getIt {
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  height: 40px;
  padding: 0 35px;
  color: #0d0d0d;
  font-size: 16px;
  font-weight: 600;
  line-height: 117.7%;
  /* 18.832px */

  border-radius: 100px;
  background: #bcff52;

  transition: 0.2s ease-in-out;
  transition-property: opacity;

  border-width: 0px;

  z-index: 1;

  &:focus {
    outline: none;

    border-width: 0px;
  }

  &.hidden {
    opacity: 0;
  }
}

.CardGrab_getIt_animBox {
  position: absolute;
  top: 0;
  left: 0;
}

.CardGrab_getIt_anim {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border: 30px solid rgba(255, 255, 255, 0);
  background: radial-gradient(rgba(255, 255, 255, 0), #ffffff);
  border-radius: 50%;
  filter: blur(0px);

  opacity: 0;
}
</style>
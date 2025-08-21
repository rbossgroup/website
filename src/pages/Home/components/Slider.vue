<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
// import { usePageTranslation } from "@/i18n";
import VueSVG from "@/components/VueSVG.vue";
import slider1 from "@/assets/img/HomeSlider/slider-1.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import slider2 from "@/assets/img/HomeSlider/slider-2.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import slider3 from "@/assets/img/HomeSlider/slider-3.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import slider4 from "@/assets/img/HomeSlider/slider-4.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import slider5 from "@/assets/img/HomeSlider/slider-5.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import slider6 from "@/assets/img/HomeSlider/slider-6.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import slider7 from "@/assets/img/HomeSlider/slider-7.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import company1 from "@/assets/img/HomeSlider/company-1.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import company2 from "@/assets/img/HomeSlider/company-2.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import company3 from "@/assets/img/HomeSlider/company-3.png?w=400;800;1600&format=webp&quality=90&as=srcset";
import company4 from "@/assets/img/HomeSlider/company-4.png?w=400;800;1600&format=webp&quality=90&as=srcset";

// import { useComponentTranslation } from "@/i18n";

// const t = useComponentTranslation("slider");

const currentSlideIndex = ref(0);

const slides = [
  {
    id: 1,
    title: "Cargo Services",
    subtitle: "Integrated Cargo Services",
    image: slider1,
    text: "We offer end-to-end solutions to streamline your supply chain. From build-up and breakdown of pallets to loading and unloading of aircraft by way of warehousing, customs clearance and transport coordination, our dedicated team ensures your cargo moves efficiently, securely, and on time.",
  },
  {
    id: 2,
    title: "E-commerce",
    subtitle: "E-Commerce Logistics Solutions",
    image: slider2,
    text: "We provide fast, reliable, and flexible logistics services tailored for the e-commerce sector. From efficient order fulfillment and secure warehousing to last-mile delivery and real-time tracking, we help your online business reach customers worldwide",
  },
  {
    id: 3,
    title: "Ramp Services",
    subtitle: "Specialized Aircraft Cleaning ",
    image: slider3,
    text: "Industrial cleaning is a fundamental process to maintain hygiene and cleanliness inside  but also outside the warehouse. With its cleaning expertise, RBoss guarantees workers’  safety and ensures to preserve compliance to health and security applicable norms.",
  },
  {
    id: 4,
    title: "Crew transportation",
    subtitle: "Reliable Crew Transportation Services",
    image: slider4,
    text: "We provide safe, timely, and comfortable transportation for your flight crew. Our professional drivers and well-maintained vehicles ensure smooth transfers between airports, hotels, and bases — helping your team stay focused and ready for every flight.",
  },
  {
    id: 5,
    title: "Facility Cleaning",
    subtitle: "Facility Cleaning Solutions",
    image: slider5,
    text: "Cleaning is a fundamental process to maintain hygiene and cleanliness inside but also outside the warehouse, hotels, offices, etc. With its cleaning expertise, RBoss guarantees customers and workers safety, and ensures to preserve compliance to health and security applicable norms.",
  },
  {
    id: 6,
    title: "Passenger Services",
    subtitle: "Passenger Services",
    image: slider6,
    text: "Building on its experience, strong collaborations, and trusted partnerships, RBoss Group also provides dedicated services for passengers — from baggage handling to check-in management.",
  },
  {
    id: 7,
    title: "More Services",
    subtitle: "Need customized solution?",
    image: slider7,
    text: "At RBoss Group, our flexibility and expertise allow us to go beyond our core offering, delivering customized solutions and additional services designed around our clients' evolving needs.",
  }
];

const goToSlide = (index: number) => {
  currentSlideIndex.value = index;
};

const getSlideClass = (index: number) => {
  const diff = index - currentSlideIndex.value;
  const totalSlides = slides.length;

  let normalizedDiff = diff;
  if (Math.abs(diff) > totalSlides / 2) {
    normalizedDiff = diff > 0 ? diff - totalSlides : diff + totalSlides;
  }

  switch (normalizedDiff) {
    case 0:
      return "slide--1";
    case -1:
      return "slide--2";
    case 1:
      return "slide--3";
    case -2:
      return "slide--4";
    case 2:
      return "slide--5";
    default:
      return "slide--hidden";
  }
};

const orderedSlides = computed(() => {
  return slides.map((slide, index) => ({
    ...slide,
    originalIndex: index,
    slideClass: getSlideClass(index),
    // isVisible: Math.abs(index - currentSlideIndex.value) <= 2 || Math.abs(index - currentSlideIndex.value) >= slides.length - 2,
  }));
});

const handleSlideClick = (slideData: any) => {
  if (slideData.originalIndex !== currentSlideIndex.value) {
    goToSlide(slideData.originalIndex);
  }
};

const isMobile = ref(false);
onMounted(() => {
  isMobile.value = window.matchMedia("(max-width: 600px)").matches;
  window.addEventListener("resize", () => {
    isMobile.value = window.matchMedia("(max-width: 600px)").matches;
  });
});
const isSlideVisible = (slideData: any) => {
  if (isMobile.value) {
    return slideData.originalIndex === currentSlideIndex.value;
  }
  return slideData.slideClass !== "slide--hidden";
};
</script>

<template>
  <section class="s-slider full-w">
    <div class="s-slider__buttons">
      <button class="btn-primary" v-for="(slide, index) in slides" :key="slide.id" :class="{ active: index === currentSlideIndex }" @click="goToSlide(index)">
        <VueSVG src="/svg/plane.svg" />
        {{ slide.title }}
      </button>
    </div>
    <div class="s-slider__content">
      <div
        v-for="slideData in orderedSlides" :key="slideData.id" :class="['slide', slideData.slideClass]"
        v-show="isSlideVisible(slideData)"
        @click="handleSlideClick(slideData)"
      >
        <div :class="['bg', `bg--${slideData.id}`]">
          <img :srcset="slideData.image" alt="" role="presentation" />
        </div>

        <p class="reset" :class="{ active: slideData.originalIndex === currentSlideIndex }">
          <span>{{ slideData.subtitle }}</span>
          {{ slideData.text }}
        </p>
      </div>
      <div class="s-slider__height"></div>
    </div>
    <div class="s-slider__trusted">
      <h5>Trusted by</h5>
      <div class="logos">
        <figure><img :srcset="company1" alt="Prime Air logo" /></figure>
        <figure><img :srcset="company2" alt="Alibaba logo" /></figure>
        <figure><img :srcset="company3" alt="Swissport logo" /></figure>
        <figure><img :srcset="company4" alt="DHL logo" /></figure>
      </div>
      <span>and more...</span>
    </div>
  </section>
</template>

<style scoped lang="scss">
@media (max-width: 600px) {
  .s-slider {
    .slide {
      position: static;
      width: 100%;
      max-width: 100%;
      aspect-ratio: unset;
      transform: none !important;
      left: unset !important;
      right: unset !important;
      margin-bottom: 20px;
      display: block;
      z-index: 1;

      &--hidden {
        display: none !important;
      }
    }
    &__content {
      display: block;
      position: static;
      height: auto;
    }
  }
}
.s-slider {
  position: relative;
  width: 100%;
  background-color: white;
  @apply px-20 md:px-60 lg:px-0 mt-40 lg:mt-150;

  &__height {
    width: 100%;
    aspect-ratio: 363/416;
    background-color: white;
    z-index: -1;
    @screen sm {
      aspect-ratio: 995/592;
    }
    @screen lg {
      max-width: 65%;
      margin-bottom: 160px;
    }
  }

  &__buttons {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    @apply gap-10 lg:gap-20 sm:justify-center;
  }

  &__content {
    position: relative;
    width: 100%;
    height: 100%;
    margin-top: 50px;
  }

  .slide {
    position: absolute;
    display: flex;
    aspect-ratio: 363/416;
    border-radius: 20px;
    top: 0;
    left: 50%;
    overflow: hidden;
    transition: all 0.5s ease;
    cursor: pointer;
    @screen sm {
      aspect-ratio: 995/592;
    }

    p {
      position: relative;
      color: white;
      padding: 30px;
      margin-top: auto;
      max-width: 650px;
      z-index: 7;
      pointer-events: none;
      line-height: 1.5;
      @apply text-neg-5-16 font-medium;
      span {
        @apply text-neg-5-18 block font-bold;
      }

      &::before {
        position: absolute;
        content: "";
        width: 300%;
        height: calc(100% + 20px);
        top: -20px;
        left: 0px;
        background: linear-gradient(to bottom, #21212100, #212121bd);
        z-index: -1;
        opacity: 0;
        transition: opacity 0.3s ease 0.3s;
      }

      &.active::before {
        opacity: 1;
      }
    }

    &--1 {
      width: 100%;
      max-width: 100%;
      z-index: 5;
      transform: translateX(-50%);
      cursor: default;
      @apply shadow-2xl lg:max-w-[65%];

      p {
        opacity: 1;
        transition: opacity 0.3s ease 0.5s;
      }
    }

    &--2,
    &--3 {
      width: 100%;
      max-width: 50%;
      z-index: 4;
      @apply shadow-md;

      &:hover {
        transform: scale(1.02);
        @apply shadow-lg;
      }

      p {
        opacity: 0;
        transition: opacity 0.3s ease;
      }
    }
    &--2 {
      transform: translate(-85%, 15%);
      &:hover {
        transform: translate(-85%, 15%) scale(1.02);
      }
    }
    &--3 {
      transform: translate(-15%, 15%);
      &:hover {
        transform: translate(-15%, 15%) scale(1.02);
      }
    }

    &--4,
    &--5 {
      width: 100%;
      max-width: 35%;
      z-index: 3;
      @apply shadow-sm;

      &:hover {
        transform: scale(1.05);
        @apply shadow-md;
      }

      p {
        opacity: 0;
        transition: opacity 0.3s ease;
      }
    }
    &--4 {
      left: 0;
      transform: translate(0%, 45%);
      &:hover {
        transform: translate(0%, 45%) scale(1.05);
      }
    }
    &--5 {
      left: unset;
      right: 0;
      transform: translate(0%, 45%);
      &:hover {
        transform: translate(0%, 45%) scale(1.05);
      }
    }

    &--hidden {
      width: 100%;
      max-width: 65%;
      z-index: 10;
      top: 0;
      left: unset;
      right: 0;
      transform: translate(100%, 45%) scale(0);
      pointer-events: none;
    }
  }

  .bg {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  &__trusted {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
    margin-top: 100px;
    @apply lg:gap-40;

    .logos {
      display: flex;
      gap: 20px;
      @apply sm:gap-30 md:gap-40 lg:gap-50 xl:gap-60 2xl:gap-65;

      figure {
        max-width: 179px;
      }
    }
    span {
      @apply text-neg-5-14 lg:text-neg-5-20 text-text-secondary font-light;
    }
  }
}

@media (max-width: 768px) {
  .s-slider {
    &__content {
      margin-top: 30px;
    }

    .slide {
      &--2,
      &--3 {
        max-width: 45%;
      }

      &--4,
      &--5 {
        max-width: 30%;
      }
    }
  }
}
</style>

<template>
  <div class="products-swiper">
    <div class="container py-5">
      <div class="header d-flex justify-content-between">
        <h2 class="fs-2 fw-blod" :class="'text-' + colorTitle">
          {{ titleSwiper }}
        </h2>
        <div class="shop-all">
          <a href="#" class="text-dark">Shop All</a>
        </div>
      </div>
      <Swiper
        :modules="modules"
        navigation
        :pagination="{ el: '.swiper-pagination', clickable: true }"
        :slides-per-view="responsiveDisplay"
        :space-between="50"
        class="products d-flex flex-wrap justify-content-between pb-5"
      >
        <swiper-slide
          class="product"
          :class="'product-' + i"
          v-for="(item, i) in products"
          :key="item.id"
        >
          <div class="img">
            <img
              width="100%"
              height="250px"
              :src="getImagePath(item.image)"
              alt=""
            />
          </div>
          <div class="product-details">
            <span>(product {{ i + 5 }}) </span>
            <span>{{ item.name }}</span>
          </div>
          <div class="rating">
            <img
              :src="
                getImagePath(`ratings/rating-${item.rating.stars * 10}.png`)
              "
              alt=""
            />
          </div>
          <div class="price">
            <del>${{ (item.priceCents / 100).toFixed(2) }}</del>
            From
            <b class="">${{ ((item.priceCents * 0.9) / 100).toFixed(2) }}</b>
          </div>

          <div
            class="variations d-flex gap-3 my-2"
            v-if="item.variation != null"
          >
            <span
              class="outer rounded-circle border border-dark"
              :class="'outer-' + i + ' ' + (j == 0 && ' active')"
              @click="ChangeVariation(i, j, vari)"
              v-for="(vari, j) in item.variation.Color"
              :key="j"
              :title="vari"
            >
              <span :style="'background: ' + vari" class="rounded-circle">
              </span>
            </span>
          </div>
          <div
            class="altr-variation"
            v-else
            style="height: 56px; width: 100%"
          ></div>
          <div
            class="btn btn-outline-warning text-dark border border-dark rounded-pill shadow-edit"
          >
            Choose Options
          </div>
        </swiper-slide>
        <div class="swiper-pagination"></div>
      </Swiper>
    </div>
  </div>
</template>
<script setup>
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from "swiper/vue";
// import Swiper core and required modules
import { Navigation, Pagination, Scrollbar } from "swiper";
import { defineProps, ref } from "vue";

let modules = [Navigation, Pagination, Scrollbar];
let props = defineProps({
  products: {
    type: Array,
  },
  titleSwiper: {
    type: String,
  },
  colorTitle: {
    type: String,
  },
  parentClass: {
    type: String,
  },
});
const getImagePath = (imgPath) => {
  return require(`@/assets/${imgPath}`);
};

let responsiveDisplay = ref(4);
const mediaQueries = [
  { query: "(max-width: 576px)", value: 1 },
  { query: "(max-width: 767px)", value: 2 },
  { query: "(max-width: 991px)", value: 3 },
  { query: "(min-width: 1199px)", value: 4 },
];
mediaQueries.forEach((mq) => {
  const mediaQueryList = window.matchMedia(mq.query);
  const updateValue = (event) => {
    if (event.matches) {
      responsiveDisplay.value = mq.value;
      // console.log(`Current value is now: ${responsiveDisplay.value}`);
    }
  };
  // Set the initial value
  updateValue(mediaQueryList);
  // Add a listener for changes
  mediaQueryList.addListener(updateValue);
});

const ChangeVariation = (item, index, color) => {
  let spansOuterVariation = document.querySelectorAll(
    `.${props.parentClass} .outer-${item}`
  );
  let imgOfProduct = document.querySelector(
    `.${props.parentClass} .product-${item} .img img`
  );
  let mainSrc = props.products[item].image;
  let mainColor = props.products[item].variation.Color[0].toLowerCase();
  color = color.toLowerCase();
  let curSrc = mainSrc.replace("products", "products/variations");
  curSrc = curSrc.replace(mainColor, color);
  // console.log(curSrc);
  if (imgOfProduct != null)
    imgOfProduct.setAttribute("src", getImagePath(curSrc));
  // console.log(color);
  // console.log(imgOfProduct);
  spansOuterVariation.forEach((ele, idx) => {
    ele.classList.remove("active");
    ele.classList.remove("false");
    if (idx == index) {
      ele.classList.add("active");
    }
  });
};
</script>

<style lang="scss">
.products-swiper {
  .swiper.products {
    .swiper-pagination {
      bottom: 0 !important;
    }
    .swiper-pagination-bullet {
      width: 13px;
      height: 13px;
    }
    .swiper-button-next {
      right: 0;
    }
    .swiper-button-prev {
      left: 0;
    }
    .swiper-button-next,
    .swiper-button-prev {
      border: 2px solid #5b5b5b;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      top: 40%;
      background-color: #fff;
      transition: 0.2s;
      &:hover {
        background-color: #fd6506;
        &::after {
          color: #fff;
        }
      }
      &::after {
        font-size: 20px;
        font-weight: 900;
        color: #000;
      }
    }
  }
  .header {
    padding: 10px 0px 40px;
    h2 {
      font-weight: 800;
    }
    .shop-all {
      height: fit-content;
      border-bottom: 1px solid #000;
      a {
        text-decoration: none;
      }
    }
  }
  .product {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 550px;
    .img {
      text-align: center;
      img {
        max-width: 250px;
      }
    }
    .product-details {
      font-size: 18px;
      letter-spacing: 0.5px;
      padding: 10px 0;
      height: 65px;
      overflow: hidden;
    }
    .rating {
      width: 40%;
      img {
        width: 100%;
      }
    }
    .price {
      font-size: 20px;
      font-weight: bold;
      padding: 15px 0;
      b {
        color: #e10600 !important;
      }
    }
    .variations {
      height: 40px;
      justify-content: center;
      span.outer {
        height: 40px;
        width: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: 0.3s;
        opacity: 0.7;
        cursor: pointer;
        &.active {
          opacity: 1;
        }
        & > span {
          display: block;
          width: 85%;
          height: 85%;
          border: 0.5px solid #03030359;
        }
      }
    }
    .btn {
      transition: 0.3s;
      margin: 20px 0px 0;
      font-size: 18px;
      font-weight: bold;
      padding: 10px 0;
      &.shadow-edit {
        box-shadow: 0px -4px 4px 0px rgba(0, 0, 0, 0.15);
      }
      &:hover {
        background-color: #007acc;
        color: #fff !important;
      }
    }
  }
  @media (max-width: 576px) {
    .product {
      width: 97%;
    }
  }
  @media (min-width: 577px) {
    .product {
      width: 47%;
    }
  }
  @media (min-width: 1199px) {
    .product {
      width: 23%;
    }
  }
}
</style>

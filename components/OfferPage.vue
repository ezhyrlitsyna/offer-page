<template>
  <div :class="`${theme}__offer-page`">
    <div :class="[`${theme}__offer-page__content`, 'container']">
      <header class="w-100">
        <div :class="`${theme}__offer-page__logo`">
          <img class="my-2" />
        </div>
      </header>
      <CourseInfoBlock :theme="theme" />
    </div>
    <PaymentDialog :theme="theme" id="paymentCardId" />
  </div>
</template>
<script>
export default {
  name: "OfferPage",
  props: {
    theme: {
      type: String,
    },
  },
};
</script>
<style lang="scss">
.light__offer-page {
  width: 100%;
  height: 100%;
  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
    background-image: url("../assets/images/light_background.jpg");
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    opacity: 0.04;
  }
  &__content {
    position: relative;
    z-index: 1;
  }
  &__logo {
    img {
      content: url("../assets/images/light_logo.png");
      width: 180px;
      height: 40px;
    }
  }
}
.dark__offer-page {
  width: 100%;
  height: 100%;
  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
    background-image: url("../assets/images/dark_background.jpg");
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
  }
  &__content {
    position: relative;
    z-index: 1;
  }
  &__logo {
    img {
      content: url("../assets/images/dark_logo.png");
    }
  }
}
</style>

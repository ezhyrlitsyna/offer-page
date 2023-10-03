<template>
  <div :class="`${theme}-wrapper`">
    <div class="course-info-block d-flex flex-md-row-reverse flex-sm-column-reverse flex-column justify-content-center align-items-center w-100">
      <div class="course-info-block__wrapper flex-row justify-content flex-sm-column w-100">
        <div :class="['d-flex w-100 align-items-center',
          `${isExpired ? 'justify-content-center' : 'justify-content-between'}`,
          `${isExpired ? 'justify-content-sm-start' : 'justify-content-sm-between'}`]"
        >
          <div class="course-info-block__price-block">
            <p class="course-info-block__price-block__trial-title">
              3-day trial for
              <span class="course-info-block__price-block__trial-price">
                {{ trialPrice }}
              </span>
            </p>
            <div class="course-info-block__price-block__prices">
              <p class="course-info-block__price-block__current_price">
                Then {{ currentPrice }}<span v-if="isExpired">/week</span>
              </p>
              <p
                v-if="!isExpired"
                class="course-info-block__price-block__old_price"
              >
                {{ oldPrice }}/week
              </p>
            </div>
          </div>
          <Timer
            :color="theme"
            v-if="!isExpired"
            @expiredTime="onExpiredTime"
          />
        </div>
        <div class="course-info-block__details d-none d-sm-flex">
          <div class="course-info-block__details__list">
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" />
              <span>
                Exclusive access to
                <span class="text-accent">350+</span>
                learning programs
              </span>
            </div>
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" />
              Personalized course plan
            </div>
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" />
              Comfy learning schedule made by you
            </div>
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" />
              <span>
                <span class="text-accent">24/7</span>
                tutor support in a secure chat
              </span>
            </div>
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" /> Lifetime access to materials
            </div>
          </div>
          <div class="secure-btn">
            <Icon name="lock" />
            <p class="ms-2">Safe & secure payment</p>
          </div>
          <div class="course-info-block__extra-info">
            $0.99 charged today. If you don't cancel at least 24 hours before
            the end of the 3-day trial period, you will automatically be charged
            the full price of $19.99/Month . You can cancel your subscription at
            any time. By continuing, you indicate that you've read and agree to
            our Terms & Conditions, Privacy Policy , Money Back , and
            Subscription Terms .
          </div>
        </div>
        <img
          class="d-none d-xxl-block rocket"
          src="../assets/images/rocket.png"
          alt="rocket"
        />
        <img
          class="d-none d-lg-block ball"
          src="../assets/images/ball.png"
          alt="ball"
        />
      </div>
      <div class="course-info-block__start-block">
        <div
          class="d-flex flex-column align-items-center align-items-sm-start justify-content-center"
        >
          <div
            class="d-flex flex-column align-items-center justify-content-start"
          >
            <h1>
              <span class="text-accent-start">Start</span> your learning journey
              <span class="text-accent-start">now</span>
            </h1>
            <p>
              Get a
              <span class="logo">
                <span class="logo__first">{{ logoFirst }}</span
                ><span class="logo__second">{{ logoSecond }}</span>
              </span>
              plan to rock self-learning
            </p>
          </div>
          <Button
            :view="theme"
            data-bs-toggle="modal"
            data-bs-target="#paymentCardId"
            title="Get my plan"
            class="payment-btn w-100"
          />
        </div>
        <div class="course-info-block__details d-sm-none">
          <div class="course-info-block__details__list">
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" />
              <span>
                Exclusive access to
                <span class="text-accent">350+</span>
                learning programs
              </span>
            </div>
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" />
              Personalized course plan
            </div>
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" />
              Comfy learning schedule made by you
            </div>
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" />
              <span>
                <span class="text-accent">24/7</span>
                tutor support in a secure chat
              </span>
            </div>
            <div :class="`course-info-block__details__list-item`">
              <Icon name="star" :color="theme" /> Lifetime access to materials
            </div>
          </div>
          <div class="secure-btn w-100">
            <Icon name="lock" />
            <p class="ms-2">Safe & secure payment</p>
          </div>
          <div class="course-info-block__extra-info">
            $0.99 charged today. If you don't cancel at least 24 hours before
            the end of the 3-day trial period, you will automatically be charged
            the full price of $19.99/Month . You can cancel your subscription at
            any time. By continuing, you indicate that you've read and agree to
            our Terms & Conditions, Privacy Policy , Money Back , and
            Subscription Terms .
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { ref } from "vue";
export default {
  name: "CourseInfoBlock",
  props: {
    theme: {
      type: String,
    },
  },
  emits: {
    openPaymentCard: null,
  },
  setup(props) {
    const trialPrice = ref("$0.99");
    const currentPrice = ref("$9.99");
    const oldPrice = ref("$39.99");
    const logoFirst = props.theme === "light" ? "Planet" : "Smart";
    const logoSecond = props.theme === "light" ? "Learn" : "Study";

    const isExpired = ref(false);
    const onExpiredTime = () => {
      currentPrice.value = oldPrice.value;
      isExpired.value = true;
    };
    return {
      trialPrice,
      currentPrice,
      logoFirst,
      logoSecond,
      oldPrice,
      isExpired,
      onExpiredTime,
    };
  },
};
</script>

<style lang="scss">
.course-info-block {
  font-family: "Proxima Nova";
  font-weight: 700;
  font-size: 16px;
  &__wrapper {
    background-color: white;
    border: 1px solid $grey;
    border-radius: 24px;

    padding: 12px;
    display: flex;
    min-height: 95px;
    max-width: 473px;
    align-items: center;
    margin-bottom: 24px;
  }
  &__price-block {
    display: flex;
    flex-direction: column;
    justify-content: center;
    &__prices {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      justify-content: center;
    }

    &__trial-title {
      font-family: "Proxima Nova";
      font-weight: 800;
      font-size: 18px;
      color: $main-black;
      margin-bottom: 8px;
    }
    &__trial-price {
      font-family: "Outfit";
      font-weight: 700;
      color: $orange;
    }
    &__current_price {
      font-family: "Outfit";
      font-size: 16px;
      font-weight: 400;
      margin: 0;
      line-height: 1.5;
      color: $main-black;
    }
    &__old_price {
      font-family: "Proxima Nova";
      font-size: 14px;
      font-weight: 600;
      line-height: 1.5;
      margin: 0;
      color: $grey;
      text-decoration: line-through;
    }
  }
  &__start-block {
    display: flex;
    flex-direction: column;
    align-items: start;
    max-width: 473px;
    margin-bottom: 24px;
    padding: 12px;
    h1 {
      font-family: "Proxima Nova";
      font-weight: 800;
      font-size: 32px;
      color: $main-black;
      line-height: 1.2;
      margin-bottom: 16px;
    }
    p {
      font-family: "Proxima Nova";
      font-weight: 600;
      font-size: 24px;
      color: $main-black;
      line-height: 1.5;
      margin-bottom: 32px;
    }
    .logo {
      font-family: "Proxima Nova";
      font-weight: 800;
      &__first {
        color: $orange;
      }
      &__second {
        color: $main-black;
      }
    }
  }
  &__details {
    display: flex;
    flex-direction: column;
    align-items: start;
    margin-top: 24px;
    &__list {
      display: flex;
      flex-direction: column;
      gap: 16px;
      font-family: "Proxima Nova";
      font-weight: 400;
      font-size: 16px;
      margin-bottom: 24px;
      &-item {
        display: flex;
        align-items: center;
        gap: 10px;
        .text-accent {
          color: $orange;
          font-weight: 700;
        }
      }
    }
    .secure-btn {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 56px;
      border: 1px solid $grey;
      color: $green;
      border-radius: 12px;
      p {
        color: inherit;
        font-family: "Proxima Nova";
        font-size: 16px;
        font-weight: 600;
        margin: 0;
        padding: 0;
      }
    }
  }
  &__extra-info {
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    font-family: "Proxima Nova";
    font-weight: 400;
    font-size: 12px;
    color: $grey;
    margin-top: 32px;
  }
}
.dark-wrapper {
  .course-info-block {
    &__wrapper {
      background-color: rgba($main-black, 0.6);
      border: none
    }
    &__price-block {
      &__trial-title,
      &__current_price,
      &__old_price {
        color: $main-white;
      }
      &__trial-price {
        color: $blue;
      }
    }
    &__start-block {
      background-color: rgba($main-black, 0.6);
      border-radius: 24px;
      h1,
      p {
        color: $main-white;
      }
      .logo {
        &__first {
          color: #4eaaff;
        }
        &__second {
          color: $main-white;
        }
      }
      .text-accent-start {
        color: #4eaaff;
      }
    }
    &__details {
      &__list {
        color: $main-white;
        &-item .text-accent {
          color: $blue;
        }
      }
    }
    .secure-btn {
      border: none;
      color: $blue;
      font-family: "Proxima Nova";
      font-weight: 600;
      background-color: rgba($main-black, 0.2);
      border-radius: 12px;
      p {
        color: inherit;
        font-family: "Proxima Nova";
        font-size: 16px;
        font-weight: 600;
        margin: 0;
        padding: 0;
      }
    }
    &__extra-info {
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      font-family: "Proxima Nova";
      font-weight: 400;
      font-size: 12px;
      color: $grey;
      margin-bottom: 24px;
    }
  }
}
@media (min-width: $desktop) {
  .light-wrapper, .dark-wrapper {
    margin-top: 65px;
  }
  .light-wrapper {
    .course-info-block__wrapper{
      position: relative;
      &::before {
        content: url("../assets/images/Asteroid.png");
        position: absolute;
        top: -55px;
        right: 10px;
      }
      .ball, .rocket {
        display: none !important;
      }
    }
  }
  .dark-wrapper {
    .course-info-block__wrapper {
      border: 4px solid rgba($main-white, 0.4);
      position: relative;
      .rocket {
        position: absolute;
        top: 55px;
        right: -180px;
      }
      .ball {
        position: absolute;
        top: -50px;
        left: -40px;
        z-index: -1;
      }
    }
    .course-info-block__start-block {
      background: none;
      h1 {
        text-transform: uppercase;
      }
    }
    .course-info-block__details__list-item:nth-child(1),
    .course-info-block__details__list-item:nth-child(4){
      color: $blue;
    }
  }
  .course-info-block {
    gap: 40px;
    &__wrapper {
      padding: 24px;
      padding-bottom: 12px;
    }
    &__price-block {
      &__trial-title {
        font-size: 24px;
      }
      &__trial-price {
        font-size: 24px;
      }
      &__current_price {
        font-size: 20px;
      }
      &__old_price {
        font-size: 16px;
      }
    }
    &__start-block {
      padding: 24px;
      padding-bottom: 12px;
      h1 {
        font-size: 48px;
      }
      p {
        font-size: 24px;
      }
      .payment-btn {
        width: 65% !important
      }
    }
    &__details {
      &__list {
        font-size: 16px;
      }
    }
    &__extra-info {
      font-size: 14px;
    }
  }
}

</style>

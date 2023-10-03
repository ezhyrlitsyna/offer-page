<template>
  <button
    :type="type"
    :class="`btn ${view ? `btn__${view}` : ''}
    ${ disabled ? `btn__${view}-disabled` : ''}`"
    :style="`background-color: ${bgColor}`"
    :disabled="disabled"
  >
    <div class="d-flex align-items-center justify-content-center">
      <Icon
        v-if="prependIcon"
        :name="prependIcon"
        style="width: fit-content; height: fit-content"
        class="me-2"
      />
      <span :style="`color: ${color};`"> {{ title }}</span>
    </div>
  </button>
</template>
<script>
export default {
  name: "Button",
  props: {
    title: {
      type: String,
      default: "",
    },
    color: {
      type: String,
      default: "",
    },
    bgColor: {
      type: String,
      default: "",
    },
    prependIcon: {
      type: String,
      default: "",
    },
    view: {
      type: String,
      default: "",
      validates: (value) => {
        return ["light", "dark", "googlePay", "paypal"].includes(value);
      },
    },
    type: {
      type: String,
      default: "button",
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
<style lang="scss">
.btn {
  height: 56px;
  width: 100%;
  border-radius: 12px;
  font-family: "Proxima Nova";
  font-weight: 700;
  font-size: 16px;
  padding: 0;
  margin: 0;
  &__light {
    background-color: $main-black;
    color: $main-white;
    width: 320px;
    color: $main-white !important;
    &:hover {
      background-color: $green;
    }
    &-disabled {
      background-color: #d2d2d2 !important;
    }
  }
  &__dark {
    background-color: $main-white;
    color: #495eee;
    width: 320px;
    &:hover {
      background-color: #6c89ff;
      color: $main-white;
    }
    &-disabled {
      background-color: $grey-dark !important;
      color: $main-white !important;
      border: none;
    }
  }
  &__paypal {
    background-color: #fec538;
    text-transform: uppercase;
    &:hover {
      background-color: #ffda80;
    }
  }
  &__googlePay {
    background-color: $main-white;
    text-transform: uppercase;
    border: 1px solid #e1e1e1;
    &:hover {
      background-color: $main-black;
      color: $main-white;
    }
  }
}
</style>

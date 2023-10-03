<template>
  <form class="credit-card-form w-100 h-100">
    <div :class="['d-flex flex-column', `${theme}`]">
      <div
        :class="[
          'credit-card-form__group d-flex justify-content-between',
          `${
            isCardNumberValid === true
              ? 'credit-card-form__group__success'
              : isCardNumberValid === false
              ? 'credit-card-form__group__error'
              : ''
          }`,
        ]"
      >
        <label for="card-number">Card number</label>
        <input
          id="card-number"
          type="text"
          @input="onCardNumberHandler"
          @blur="hideCardNumbers"
          @focus="showCardNumbers"
          maxlength="19"
        />
        <div
          v-if="isCardNumberValid === false"
          class="credit-card-form__group__error-message"
        >
          Invalid card
        </div>
        <Icon name="paymentCard" style="width: fit-content" />
      </div>
    </div>
    <div class="d-flex w-100 justify-content-between">
      <Dropdown
        :theme="theme"
        :value="form.month"
        :items="monthList"
        label="Month"
        class="item-30"
        @update:modelValue="form.month = $event"
      />
      <Dropdown
        :theme="theme"
        :value="form.year"
        :items="yearList"
        label="Year"
        class="item-30"
        @update:modelValue="form.year = $event"
      />
      <div class="credit-card-form__group item-30">
        <label for="cvc">CVC</label>
        <input
          id="cvc"
          type="text"
          maxlength="3"
          @focus="showCvc"
          @blur="hideCvc"
          @input="onCvcHandler"
        />
      </div>
    </div>
  </form>
</template>
<script>
import { ref, watch } from "vue";

export default {
  name: "CreditCardForm",
  props: {
    theme: {

    }
  },
  emits: {
    formValid: null,
  },
  setup(props, { emit }) {
    const cardNumber = ref("");
    const isCardNumberValid = ref(null);
    const isFormValid = ref(false);

    const monthList = [
      "January",
      "February",
      "March",
      "April",
      "May",
      "June",
      "July",
      "August",
      "September",
      "October",
      "November",
      "December",
    ];

    const yearList = [
      "2023",
      "2024",
      "2025",
      "2026",
      "2027",
      "2028",
      "2029",
      "2030",
    ];

    const form = ref({
      cardNumber: "",
      month: "",
      year: "",
      cvc: "",
    });

    const mask = (value, limit, separator) => {
      var output = [];
      for (let i = 0; i < value.length; i++) {
        if (i !== 0 && i % limit === 0) {
          output.push(separator);
        }
        output.push(value[i]);
      }
      return output.join("");
    };

    const unmask = (value) => value.replace(/[^\d]/g, "");

    const onCardNumberHandler = (e) => {
      let regex = /^\d{0,16}$/g;
      let newValue = unmask(e.target.value);
      let separatorsAmount = Math.floor(e.target.selectionEnd / 5);
      let newCursorPosition = e.target.selectionEnd + separatorsAmount;

      if (newValue.match(regex).length) {
        if (newValue.length === 16) {
          isCardNumberValid.value = true;
        }
        newValue = mask(newValue, 4, " ");
        e.target.value = newValue;
        form.value.cardNumber = newValue;
      }
      e.target.setSelectionRange(newCursorPosition, newCursorPosition);
    };

    const hideCardNumbers = (e) => {
      validateCardNumber(e.target.value);
      e.target.value = form.value.cardNumber.replace(/\d/g, "•");
    };

    const showCardNumbers = (e) => {
      if (unmask(form.value.cardNumber).length === 16)
        isCardNumberValid.value = true;
      e.target.value = form.value.cardNumber;
    };

    const validateCardNumber = (number) => {
      if (unmask(number)?.length === 16 || unmask(number)?.length === 0) {
        isCardNumberValid.value = null;
      } else {
        isCardNumberValid.value = false;
      }
    };

    const onCvcHandler = (e) => {
      let regex = /^\d{0,3}$/g;
      let newValue = unmask(e.target.value)
      if (!newValue.match(regex).length) {
        return;
      } else {
        e.target.value = newValue;
        form.value.cvc = e.target.value;
      }
    };

    const showCvc = (e) => {
      e.target.value = form.value.cvc;
    };

    const hideCvc = (e) => {
      e.target.value = form.value.cvc.replace(/\d/g, "•");
    };

    watch(
      () => form.value.cardNumber,
      () => {
        if (unmask(form.value.cardNumber).length === 0)
          isCardNumberValid.value = null;
      },
      { deep: true }
    );
    watch(
      () => form.value,
      () => {
        if (
          unmask(form.value.cardNumber).length === 16 &&
          form.value.month &&
          form.value.year &&
          form.value.cvc.length === 3
        ) {
          isFormValid.value = true;
          emit("formValid", true);
        } else {
          isFormValid.value = false;
          emit('formValid', false)
        }
      },
      {
        deep: true,
      }
    );

    return {
      form,
      monthList,
      yearList,
      cardNumber,
      isFormValid,
      isCardNumberValid,

      onCardNumberHandler,
      hideCardNumbers,
      showCardNumbers,
      showCvc,
      hideCvc,
      onCvcHandler,
    };
  },
};
</script>
<style lang="scss">
.credit-card-form {
  display: flex;
  flex-direction: column;
  gap: 30px;
  &__group {
    position: relative;
    height: 48px;
    padding: 12px;
    border: 1px solid $grey;
    border-radius: 8px;
    > label {
      position: absolute;
      top: -6px;
      left: 12px;
      padding: 0 5px;
      border-radius: 12px;
      background-color: $main-white;
      font-family: "Proxima Nova";
      font-weight: 700;
      font-size: 12px;
      color: $text-grey;
    }
    > input {
      font-size: "Proxima Nova";
      font-weight: 700;
      font-size: 14px;
      width: 100%;
      border: none;
      color: $text-grey;
    }
    &__success {
      border-color: $green;
    }
    &__error {
      border-color: $red;
      &-message {
        font-family: "Proxima Nova";
        font-weight: 600;
        font-size: 11px;
        color: $red;
        display: flex;
        align-items: center;
        margin-right: 4px;
        text-wrap: nowrap;
      }
    }
  }
  .item-30 {
    width: 30%;
  }
}
.dark {
  .credit-card-form__group {
    border-color: transparent;
    background-color: $grey-dark;
    label {
      background-color: $grey-dark;
      color: $main-white;
    }
    input {
      background-color: transparent;
      color: $main-white;
    }
    &__success {
      border-color: #7899FF;
    }
    &__error {
      border-color: $red;
    }
  }

}
</style>

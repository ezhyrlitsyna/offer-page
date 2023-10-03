<template>
  <div class="modal"  :id="id" tabindex="-1" data-bs-backdrop="static" data-bs-keyboard="false" aria-hidden="true" >
    <div :class="['modal-dialog modal-dialog-centered', `${theme}`]">
      <div class="modal-content">
        <div class="modal-header p-0">
          <h5 class="modal-title">Payment method</h5>
        </div>
        <div class="modal-body p-0">
          <Button view="paypal" prependIcon="paypal" title="buy now" />
          <Button view="googlePay" prependIcon="google" title="pay" />
          <div class="modal-divider">
            <hr class="modal-divider-line" />
            <span>or</span>
            <hr class="modal-divider-line" />
          </div>
          <CreditCardForm :theme="theme"  @formValid="onValidateCreditCard" />
        </div>
        <div class="modal-footer d-flex flex-column p-0">
          <Button
            :view="theme"
            :disabled="!isCanSubmit"
            title="Save changes"
            class="mb-1 btn-90"
          />
          <Button
            :color="theme === 'light' ? 'grey': 'white'"
            class="btn-90"
            bgColor="transparent"
            data-bs-dismiss="modal"
            title="Close"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "PaymentDialog",
  props: {
    id: {
      type: String,
      default: "",
    },
    theme: {
      type: String,
      default: "",
    },
  },
  setup() {
    const isCanSubmit = ref(false);
    const onValidateCreditCard = (isValid) => {
      isCanSubmit.value = isValid;
    };

    return {
      isCanSubmit,
      onValidateCreditCard,
    };
  },
};
</script>

<style lang="scss">
.modal {
  &-content {
    max-width: 400px;
    padding: 24px;
    padding-bottom: 12px;
  }
  &-header {
    border: none;
    justify-content: center;
    margin-bottom: 24px;
  }
  &-title {
    font-family: "Proxima Nova";
    font-weight: 800;
    font-size: 21px;
  }
  &-body {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 16px;
    margin-bottom: 40px;
  }
  &-footer {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border: none;
    .btn-90 {
      width: 90% !important;
    }
  }
  &-divider {
    width: 100%;
    color: $grey;
    text-transform: uppercase;
    display: flex;
    gap: 8px;
    align-items: center;
    &-line {
      border: none;
      background-color: rgba($main-black, 0.1);
      height: 2px;
    }
    span {
      font-family: "Proxima Nova";
      font-weight: 600;
      font-size: 12px;
      color: $grey;
    }
  }
}
.dark {
  .modal-content {
    background-color: #4D5E82;
    color: $main-white;
  }
  .modal-divider hr {
    background-color: rgba($main-white, 1);
  }
}
</style>

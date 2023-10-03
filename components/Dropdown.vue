<template>
  <div :class="`${theme}-custom-dropdown`">
    <div class="dropdown" data-bs-toggle="dropdown">
      <div
				v-if="label" class="dropdown__label"
			>
				{{ label }}
			</div>
      <p>{{ value || defaultValue }}</p>
      <Icon
				v-if="!value" name="arrowDown"
			/>
      <ul
				class="dropdown-menu" aria-labelledby="dropdownMenuButton1"
			>
        <li
          v-for="(item, index) in items"
          :key="`${item}-${index}`"
          class="dropdown-item"
          :class="value === item ? 'dropdown-item-active' : ''"
          @click="onSelectItem(item)"
        >
          {{ item }}

          <Icon
						v-if="value === item"
						name="check"
					/>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import { ref } from "vue";
export default {
  name: "Dropdown",
  props: {
		value: {
			type: String,
			default: "",
		},
    label: {
      type: String,
      default: "",
    },
    items: {
      type: Array,
      default: () => [],
    },
    theme: {
      type: String,
      default: "",
    },
  },
	emits: {
		'update:modelValue': null
	},
  setup(props, {emit}) {
    const defaultValue = ref("Select");

    const onSelectItem = (item) => {
			emit('update:modelValue', item)
    };

    return {
      defaultValue,
      onSelectItem,
    };
  },
};
</script>
<style lang="scss">
.dropdown {
  position: relative;
	height: 48px;
  display: flex;
  justify-content: space-between;
  padding: 12px;
  align-items: center;
  border: 1px solid $grey;
  border-radius: 8px;
  font-family: "Proxima Nova";
  font-weight: 400;
  font-size: 12px;
  color: $text-grey;
  &-toggle {
    border: none;
    &::after {
      border: none;
    }
  }
  &-menu {
    transform: none !important;
    border-color: $grey;
    position: absolute;
    inset: calc(100% + 1px) auto auto 0 !important;
    width: 100% !important;
    border-radius: 0 0 8px 8px;
    --bs-dropdown-min-width: 0px;
    --bs-dropdown-padding-y: 0;
  }
  &-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-family: "Proxima Nova";
    font-weight: 400;
    font-size: 12px;
    color: $text-grey;
    padding: 8px;
    border-bottom: 1px solid $grey;
    &:hover {
      background-color: $green;
      color: $main-white;
    }
    &-active {
      background-color: $green !important;
      color: $main-white;
    }
  }
  &__label {
    position: absolute;
    top: -6px;
    left: 12px;
    padding: 0 5px;
    border-radius: 12px;
    background-color: $main-white;
    font-family: "Proxima Nova";
    font-weight: 700;
    font-size: 12px;
  }
  &__title {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px;
    color: $text-grey;
  }
}
.dark-custom-dropdown {
  .dropdown {
    border: transparent;
    color: $main-white;
    background-color: $grey-dark;
  }
  .dropdown__label {
    background-color: $grey-dark;
  }
  .dropdown-menu {
    background-color: $grey-dark;
    border-color: rgba($main-white, 0.3);
  }
  .dropdown-item {
    border-bottom: 1px solid rgba($main-white, 0.3);
    color: $main-white;
    &:hover {
      background-color: #6C89FF;
      color: $main-white;
    }
    &-active {
      background-color: #6C89FF !important;
      color: $main-white;
    }
  }
}
</style>

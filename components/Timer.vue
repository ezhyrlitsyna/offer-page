<template>
  <div :class="['progress rounded-3 timer', { 'timer-attention' : redBorderIsVisible}]">
    <div
			ref="progressBar"
      :class="`progress-bar rounded-3 timer-${color}`"
			:style="{ width: `${progressBarLeftWidth}%` }"
      role="progressbar"
      aria-valuemin="0"
      aria-valuemax="100"
    />
    <span class="timer-title">
			{{ formattedMinutes }} : {{ formattedSeconds }}
		</span>
  </div>
</template>
<script>
import { computed, ref, onMounted } from "vue";
export default {
  name: "Timer",
  props: {
    minutes: {
      type: Number,
      default: 3,
    },
    color: {
      type: String,
      default: "light",
    },
  },
  emits: {
    expiredTime: null,
  },
  setup(props, { emit }) {
    const formatTime = (time) => {
      return time && time < 10 ? `0${time}` : time || "00";
    };

    const formattedMinutes = ref(formatTime(props.minutes) || "00");
    const formattedSeconds = ref("00");
		const progressBarLeftWidth = ref(100);
		const redBorderIsVisible = ref(false);

		const countProgressBarWidth = () => {
			progressBarLeftWidth.value = (storage.value?.getItem("timer") * 100) / (props.minutes * 60);
		}

    const storage = computed(() => {
      return process.client ? window.localStorage : null;
    });

    const countDownTimer = () => {
      let countTimer = storage.value?.getItem("timer");
      if (countTimer <= 0) {
        emit("expiredTime");
        clearInterval(timer);
      } else {
        countTimer--;
        storage.value?.setItem("timer", countTimer);
      }
      formattedMinutes.value = formatTime(parseInt(countTimer / 60));
      formattedSeconds.value = formatTime(parseInt(countTimer % 60));

			if (countTimer <= 10 && countTimer > 0) {
				redBorderIsVisible.value = true;
			} else {
				redBorderIsVisible.value = false;
			}
			countProgressBarWidth();
    };

    const timer = setInterval(countDownTimer, 1000);

    onMounted(() => {
      if (!storage.value?.getItem("timer")) {
        storage.value?.setItem("timer", props.minutes * 60);
      }
    });

    return {
      formattedMinutes,
      formattedSeconds,
			progressBarLeftWidth,
			redBorderIsVisible
    };
  },
};
</script>
<style lang="scss">
.timer {
  position: relative;
  width: 110px;
  height: 47px;
  background-color: $main-black;
  &-title {
    font-family: "Proxima Nova";
    font-weight: 600;
    font-size: 24px;
    color: $main-white;
    line-height: 1.5;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
	&-attention {
		border: 2px solid $red;
		box-shadow: 0 0 6px rgba($red, 0.8);
	}
  &-light {
    background-color: $green;
  }
  &-dark {
    background-color: #4EAAFF;
  }
}
@media (min-width: $desktop) {
  .timer{
    width: 160px;
    height: 60px;
  }
}
</style>

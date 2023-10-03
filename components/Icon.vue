<template>
	<div :class="`icon icon__${color}`" v-html="require(`../assets/svg/${name}.svg?.raw`)"/>
</template>
<script>
export default {
	name: 'Icon',
	props: {
		name: {
			type: String,
			default: '',
		},
		color: {
			type: String,
			default: '',
		},
	},
}
</script>
<style lang="scss">
.icon {
	display: flex;
	align-items: center;
	justify-content: center;
	width: fit-content;
	height: fit-content;
	&__light {
		color: $orange;
	}
	&__dark {
		color: $blue;
	}
}
</style>
<template>
    <component :is="componentName" />
</template>

<script>
export default {
  name: 'IndexPage',
  computed: {
    componentName: function() {
      let name = this.queryParams || 'LightPage'
      return name
    },
    randomTheme: function() {
      return this.themeList[Math.floor(Math.random() * this.themeList.length)];
    }
  },
  async asyncData(context) {
    return {
    themeList: ["LightPage", "DarkPage"],
    queryParams: context.query.abtest
    }
  },
  mounted() {
    this.$router.replace({ query: {abtest: this.randomTheme}})
  },
  watch: {
    $route (to, from) {
      this.queryParams = to.query.abtest
    }
  }
}
</script>
<style lang="scss">
</style>


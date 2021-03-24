<template>
  <div v-if="currentPage === 'Dashboard'">
    <h1>Dashboard</h1>
  </div>
</template>

<script>
export default {
  name: "Dashboard",
  props: ['currentPage'],
  data() {
    return {}
  },
  methods: {

  }
}
</script>

<style>

</style>
<template>
  <div class="page-title">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: 'PageTitle',
  props: {
    msg: String
  }
}
</script>

<style scoped>
h1 {
  text-align: center;
  color: #ffffff;
}
</style>

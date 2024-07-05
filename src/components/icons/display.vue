<template>
  <div class="display">{{ value }}</div>
</template>

<script>
export default {
  props: ['value']
}
</script>

<style>
.display {
  background: #333;
  color: #fff;
  font-size: 2em;
  padding: 10px;
  text-align: right;
}
</style>

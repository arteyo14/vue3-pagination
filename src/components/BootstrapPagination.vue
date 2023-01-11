<template>
  <nav aria-label="Page navigation example">
    <ul class="pagination flex-wrap">
      <li
        class="page-item"
        v-for="num of page"
        :class="{ active: num === modelValue }"
      >
        <a class="page-link" @click="$emit('update:model-value', num)">{{
          num
        }}</a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  props: {
    page: {
      type: Number,
      required: true,
    },
    modelValue: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style scoped>
a {
  cursor: pointer;
}
</style>

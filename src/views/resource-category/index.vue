<template>
  <div class="resourceCategory">
    <resource-category-list />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import resourceCategoryList from './components/ResourceCategoryList.vue'
export default Vue.extend({
  name: 'resourceCategoryIndex',
  components: {
    resourceCategoryList
  }
})
</script>

<style lang="scss" scoped></style>

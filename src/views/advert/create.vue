<template>
  <div class="course-create">
    <create-or-update />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import CreateOrUpdate from './components/CreateOrUpdate.vue'
export default Vue.extend({
  name: 'advert-create',
  components: {
    CreateOrUpdate
  }
})
</script>

<style lang="scss" scoped>
</style>

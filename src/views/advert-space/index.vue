<template>
  <div class="advert-space">
    <advert-space-list />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import advertSpaceList from './components/AdvertSpaceList.vue'

export default Vue.extend({
  name: 'AdvertSpaceIndex',
  components: {
    advertSpaceList
  }
})
</script>

<style lang="scss" scoped></style>

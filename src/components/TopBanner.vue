<template>
  <div class="top-banner bg-primary text-white text-center py-4 px-4 d-flex justify-content-between align-items-center">
    <h1 class="mb-0">
      {{ bannerText }}
    </h1>
    <button @click="resetGroupNumber" class="btn btn-light">
      <i class="bi bi-gear"></i> Grup numarasını sıfırla
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue'

export default defineComponent({
  name: 'TopBanner',
  props: {
    bannerText: {
      type: String as PropType<string>,
      default: "Welcome to Pakachu's Grand Tournament!"
    }
  },
  emits: ['reset'],
  methods: {
    resetGroupNumber() {
      this.$emit('reset')
    }
  }
})
</script>

<style scoped>
/* Additional styling if needed */
</style>

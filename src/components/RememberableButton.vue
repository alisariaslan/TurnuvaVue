<template>
    <div class="d-flex justify-content-center mb-4">
        <button class="btn btn-primary btn-lg" @click="count++">Count is: {{ count }}</button>
      </div>
</template>

<script lang="ts">
import { ref, onMounted, watch, defineComponent } from 'vue'

export default defineComponent({
  setup() {
    // localStorage'dan count değerini al, yoksa 0 olarak başlat
    const count = ref(parseInt(localStorage.getItem('count') || '0'))

    // count değişkenini izleyerek her değişiklikte localStorage'a kaydet
    watch(count, (newValue) => {
      localStorage.setItem('count', newValue.toString())
    })

    // Sayfa yüklendiğinde localStorage'daki değeri al ve count'u ayarla
    onMounted(() => {
      const savedCount = localStorage.getItem('count')
      if (savedCount !== null) {
        count.value = parseInt(savedCount)
      }
    })

    return {
      count
    }
  }
})
</script>

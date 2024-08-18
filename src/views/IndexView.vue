<template>
  <div>
    <!-- Top Banner Section -->
    <header>
      <TopBanner bannerText="Pakachu's Grand Tournament - Turnuva Aracı" @reset="resetGroupNumber" />
    </header>

    <main>
      <div class="container py-4 px-4">
        <!-- Group 1 -->
        <CompetitorList
          ref="group1A"
          storageKey="group1_a"
          :headerText="`${groupNumber}. Grup (Herkes)`"
          @transfer="handleTransfer('group1_a', 'group1_b')"
        />
        <MatchList
          ref="group1B"
          storageKey="group1_b"
          :headerText="`${groupNumber}. Grup (Eşleştirilenler)`"
        />
        <CompetitorList
          ref="group1C"
          storageKey="group1_c"
          :headerText="`${groupNumber}. Grup (Kaybedenler)`"
          @transfer="handleTransfer('group1_c', 'group1_b')"
        />
        <CompetitorList
          ref="group1D"
          storageKey="group1_d"
          :headerText="`${groupNumber}. Grup (Kazananlar)`"
          footerText="Aktar"
          @transfer="handleTransferAndIncrementGroupNumber('group1_d', 'group1_a')"
        />
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import TopBanner from '@/components/TopBanner.vue'
import CompetitorList from '../components/CompetitorList.vue'
import MatchList from '@/components/MatchList.vue'

export default defineComponent({
  components: {
    CompetitorList,
    MatchList,
    TopBanner
  },
  setup() {
    const groupNumber = ref<number>(parseInt(localStorage.getItem('groupNumber') || '1', 10))
    const group1A = ref<InstanceType<typeof CompetitorList> | null>(null)
    const group1B = ref<InstanceType<typeof MatchList> | null>(null)
    const group1C = ref<InstanceType<typeof CompetitorList> | null>(null)
    const group1D = ref<InstanceType<typeof CompetitorList> | null>(null)

    const handleTransfer = (sourceKey: string, targetKey: string) => {
      const sourceItems = JSON.parse(localStorage.getItem(sourceKey) || '[]')
      const targetItems = JSON.parse(localStorage.getItem(targetKey) || '[]')

      const shuffleArray = (array: any[]) => {
        for (let i = array.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1))
          ;[array[i], array[j]] = [array[j], array[i]]
        }
        return array
      }

      const shuffledItems = shuffleArray(sourceItems)

      while (shuffledItems.length >= 2) {
        const pair = [shuffledItems.shift(), shuffledItems.shift()]
        targetItems.push(pair)
      }

      localStorage.setItem(sourceKey, JSON.stringify(sourceItems))
      localStorage.setItem(targetKey, JSON.stringify(targetItems))

      if (group1A.value) group1A.value.fetchItems()
      if (group1B.value) group1B.value.fetchItems()
      if (group1C.value) group1C.value.fetchItems()
    }

    const handleTransferAndIncrementGroupNumber = (sourceKey: string, targetKey: string) => {
      const sourceItems = JSON.parse(localStorage.getItem(sourceKey) || '[]')
      const targetItems = JSON.parse(localStorage.getItem(targetKey) || '[]')

      if(sourceItems.length === 0) {
        return
      }

      // Transfer items from source to target
      while (sourceItems.length > 0) {
        const item = sourceItems.shift()
        if (item) {
          targetItems.push(item)
        }
      }

      localStorage.setItem(sourceKey, JSON.stringify(sourceItems))
      localStorage.setItem(targetKey, JSON.stringify(targetItems))

      // Increment group number
      groupNumber.value += 1
      localStorage.setItem('groupNumber', groupNumber.value.toString())

      if (group1A.value) group1A.value.fetchItems()
      if (group1B.value) group1B.value.fetchItems()
      if (group1C.value) group1C.value.fetchItems()
      if (group1D.value) group1D.value.fetchItems()
    }

    const resetGroupNumber = () => {
      groupNumber.value = 1
      localStorage.setItem('groupNumber', '1')

      // Optionally, you might want to refresh the components or perform other actions
      if (group1A.value) group1A.value.fetchItems()
      if (group1B.value) group1B.value.fetchItems()
      if (group1C.value) group1C.value.fetchItems()
      if (group1D.value) group1D.value.fetchItems()
    }

    return {
      groupNumber,
      handleTransfer,
      handleTransferAndIncrementGroupNumber,
      resetGroupNumber,
      group1A,
      group1B,
      group1C,
      group1D
    }
  }
})
</script>

<style scoped>
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 0px 0px;
  grid-template-areas:
    '. . . .'
    '. . . .';
  max-width: 100%;
  gap: 50px 20px;
}
</style>

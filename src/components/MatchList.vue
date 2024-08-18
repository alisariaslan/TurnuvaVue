<template>
  <!-- Item Management Section -->
  <div class="card shadow-sm">
    <div class="card-header">
      <h5 class="mb-0">
        {{ headerText }}
      </h5>
    </div>
    <div class="card-body">
      <!-- Add Item Input -->
      <div class="input-group mb-3">
        <input
          v-model="newItem1"
          type="text"
          class="form-control"
          placeholder="Katılımcı 1"
          aria-label="Katılımcı 1"
        />
        <input
          v-model="newItem2"
          type="text"
          class="form-control"
          placeholder="Katılımcı 2"
          aria-label="Katılımcı 2"
        />
        <button class="btn btn-success" @click="addItem">Ekle</button>
      </div>
      <!-- Item List -->
      <ul class="list-group">
        <li
          v-for="(pair, index) in items"
          :key="index"
          class="list-group-item d-flex align-items-center"
        >
          <div class="row w-100">
            <!-- Katılımcı 1 -->
            <div class="col text-end">
              {{ pair[0] }}
            </div>
            <!-- Matching Icon -->
            <div class="col-auto text-center">
              <i class="bi bi-arrow-right"></i>
              <!-- Use Bootstrap Icons or your preferred icon -->
            </div>
            <!-- Katılımcı 2 -->
            <div class="col text-start">
              {{ pair[1] }}
            </div>
          </div>
          <button class="btn btn-danger btn-sm ms-2" @click="removeItem(index)">Sil</button>
        </li>
      </ul>
    </div>
    <!-- <div class="card-footer">
      <button class="btn btn-primary" @click="matchAndTransfer()">Eşleştir ve Aktar</button>
    </div> -->
  </div>
</template>

<script lang="ts">
import { ref, onMounted, watch, defineComponent, type PropType } from 'vue'

export default defineComponent({
  name: 'MatchList',
  props: {
    headerText: {
      type: String as PropType<string>,
      default: 'Eşleştirilenler'
    },
    storageKey: {
      type: String as PropType<string>,
      required: true
    }
  },
  setup(props) {
    const newItem1 = ref('');
    const newItem2 = ref('');
    const items = ref<[string, string][]>([]);

    const fetchItems = () => {
      const storedItems = localStorage.getItem(props.storageKey);
      if (storedItems) {
        items.value = JSON.parse(storedItems);
      }
    };

    onMounted(fetchItems);
    
    watch(() => props.storageKey, fetchItems);

    const addItem = () => {
      if (newItem1.value.trim() !== '' && newItem2.value.trim() !== '') {
        items.value.push([newItem1.value, newItem2.value]);
        newItem1.value = '';
        newItem2.value = '';
        localStorage.setItem(props.storageKey, JSON.stringify(items.value));
      }
    };

    const removeItem = (index: number) => {
      items.value.splice(index, 1);
      localStorage.setItem(props.storageKey, JSON.stringify(items.value));
    };

    return {
      newItem1,
      newItem2,
      items,
      addItem,
      removeItem,
      fetchItems
    };
  }
});
</script>

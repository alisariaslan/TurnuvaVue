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
          v-model="newItem"
          type="text"
          class="form-control"
          placeholder="Katılımcı adı"
          aria-label="Katılımcı adı"
        />
        <button class="btn btn-success" @click="addItem">Ekle</button>
      </div>
      <!-- Item List -->
      <ul class="list-group">
        <li
          v-for="(item, index) in items"
          :key="index"
          class="list-group-item d-flex justify-content-between align-items-center"
        >
          {{ item }}
          <button class="btn btn-danger btn-sm" @click="removeItem(index)">Sil</button>
        </li>
      </ul>
    </div>
    <div class="card-footer">
      <button class="btn btn-primary" @click="$emit('transfer')">
        {{ footerText }}
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, onMounted, watch, defineComponent, type PropType } from 'vue';

export default defineComponent({
  name: 'CompetitorList',
  props: {
    headerText: {
      type: String as PropType<string>,
      default: 'Katılımcılar'
    },
    footerText: {
      type: String as PropType<string>,
      default: 'Eşleştir'
    },
    storageKey: {
      type: String as PropType<string>,
      required: true
    }
  },
  setup(props) {
    const newItem = ref('');
    const items = ref<string[]>([]);

    const fetchItems = () => {
      const storedItems = localStorage.getItem(props.storageKey);
      if (storedItems) {
        items.value = JSON.parse(storedItems);
      }
    };

    onMounted(fetchItems);
    
    watch(() => props.storageKey, fetchItems);

    const addItem = () => {
      if (newItem.value.trim() !== '') {
        items.value.push(newItem.value);
        newItem.value = '';
        localStorage.setItem(props.storageKey, JSON.stringify(items.value));
      }
    };

    const removeItem = (index: number) => {
      items.value.splice(index, 1);
      localStorage.setItem(props.storageKey, JSON.stringify(items.value));
    };

    return {
      newItem,
      items,
      addItem,
      removeItem,
      fetchItems
    };
  }
});
</script>


<template>
  <div> <div>
      <h2>Задача 1. Стани сторінки</h2>
      
      <div style="margin-bottom: 15px;">
        <button @click="isLoading = !isLoading">Toggle loading</button>
        <button @click="hasError = !hasError">Toggle error</button>
        <button @click="items = []">Clear items</button>
        <button @click="items = ['Елемент 1', 'Елемент 2']">Add sample items</button>
      </div>

      <div style="border: 1px solid #ccc; padding: 10px;">
        <p v-if="isLoading">Завантаження...</p>
        <p v-else-if="hasError">Помилка завантаження</p>
        <p v-else-if="items.length === 0">Немає даних</p>
        <ul v-else>
          <li v-for="(item, index) in items" :key="index">{{ item }}</li>
        </ul>
      </div>
    </div>

    <div>
      <h2>Задача 2. Приховування панелі (v-if vs v-show)</h2>
      
      <button @click="isPanelVisible = !isPanelVisible" style="margin-bottom: 15px;">
        Показати/Сховати панель
      </button>
      
      <div style="display: flex; gap: 20px;">
        <div v-if="isPanelVisible" style="padding: 10px; background: #e3f2fd; border: 1px solid blue;">
          <strong>v-if:</strong> Цей елемент повністю видаляється з DOM.
        </div>
        
        <div v-show="isPanelVisible" style="padding: 10px; background: #f3e5f5; border: 1px solid purple;">
          <strong>v-show:</strong> Цей елемент залишається в DOM (змінюється display).
        </div>
      </div>
    </div>

    <div>
      <h2>Задача 3. Список карток і :key</h2>
      
      <button @click="addProduct" style="margin-bottom: 15px;">Додати товар</button>

      <ul>
        <li 
          v-for="p in products" 
          :key="p.id"
          :style="{ backgroundColor: p.category === 'Електроніка' ? '#e0f7fa' : 'transparent', padding: '5px', marginBottom: '5px', border: '1px solid #ddd' }"
        >
          <strong>{{ p.title }}</strong> (Категорія: {{ p.category }})
          <button @click="removeProduct(p.id)" style="margin-left: 10px;">Видалити</button>
        </li>
      </ul>
    </div>

    <div>
      <h2>Задача 4. Фільтрація та стани списку</h2>
      
      <div style="margin-bottom: 15px;">
        <label>Фільтр за категорією: </label>
        <select v-model="filter">
          <option value="all">Всі категорії</option>
          <option value="Електроніка">Електроніка</option>
          <option value="Меблі">Меблі</option>
          <option value="Одяг">Одяг</option>
        </select>
      </div>

      <p style="color: #666;">Показано {{ filteredProducts.length }} із {{ products.length }}</p>

      <div v-if="filteredProducts.length === 0" style="color: red; font-weight: bold;">
        Нічого не знайдено
      </div>

      <ul v-else>
        <li 
          v-for="p in filteredProducts" 
          :key="p.id"
          style="padding: 5px; border-bottom: 1px solid #eee;"
        >
          <strong>{{ p.title }}</strong> — {{ p.category }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
// ВИПРАВЛЕНО: додано імпорт computed
import { ref, computed } from 'vue'

const isLoading = ref(false)
const hasError = ref(false)
const items = ref(['Елемент 1', 'Елемент 2'])
const isPanelVisible = ref(true)

const products = ref([
  { id: 1, title: 'Ноутбук', category: 'Електроніка' },
  { id: 2, title: 'Стілець', category: 'Меблі' },
  { id: 3, title: 'Смартфон', category: 'Електроніка' },
  { id: 4, title: 'Футболка', category: 'Одяг' }
])

const addProduct = () => {
  products.value.push({
    id: Date.now(),
    title: `Новий товар ${Math.floor(Math.random() * 100)}`,
    category: 'Меблі'
  })
}

const removeProduct = (id) => {
  products.value = products.value.filter(p => p.id !== id)
}

const filter = ref('all')

const filteredProducts = computed(() => {
  if (filter.value === 'all') return products.value
  return products.value.filter(p => p.category === filter.value)
})
</script>
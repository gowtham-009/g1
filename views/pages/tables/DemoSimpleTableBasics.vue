<script setup lang="ts">
import { ref } from 'vue'

interface Dessert {
  dessert: string
  calories: number
  fat: number
  carbs: number
  protein: number
}

const desserts: Dessert[] = [
  { dessert: 'Frozen Yogurt', calories: 159, fat: 6, carbs: 24, protein: 4 },
  { dessert: 'Ice cream sandwich', calories: 237, fat: 9, carbs: 37, protein: 4 },
  { dessert: 'Eclair', calories: 262, fat: 16, carbs: 24, protein: 6 },
  { dessert: 'Cupcake', calories: 305, fat: 4, carbs: 49, protein: 3 },
  { dessert: 'Gingerbread', calories: 356, fat: 16, carbs: 49, protein: 2 },
]

const dialog = ref(false)
const selectedItem = ref<Dessert | null>(null)

const openDialog = (item: Dessert) => {
  selectedItem.value = item
  dialog.value = true
}

const isOpen = ref(false)

const openMenu = () => {
  isOpen.value = true
}

const closeMenu = () => {
  isOpen.value = false
}
</script>

<template>
  <VTable>
    <thead>
      <tr>
        <th class="text-uppercase">
          Desserts (100g Servings)
        </th>
        <th class="text-uppercase text-center">
          Calories
        </th>
        <th class="text-uppercase text-center">
          Fat (g)
        </th>
        <th class="text-uppercase text-center">
          Carbs (g)
        </th>
        <th class="text-uppercase text-center">
          Protein (g)
        </th>
        <th class="text-uppercase text-center">
          VIEW
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="item in desserts"
        :key="item.dessert"
      >
        <td>{{ item.dessert }}</td>
        <td class="text-center">
          {{ item.calories }}
        </td>
        <td class="text-center">
          {{ item.fat }}
        </td>
        <td class="text-center">
          {{ item.carbs }}
        </td>
        <td class="text-center">
          {{ item.protein }}
        </td>
        <td class="text-center">
          <div class="text-center">
            <VBtn @click="() => { openDialog(item); openMenu(); }">
              <VIcon icon="$view" />
            </VBtn>
          </div>
        </td>
      </tr>
    </tbody>
  </VTable>

  <div
    class="offcanvas"
    :class="{ 'offcanvas-open': isOpen }"
  >
    <div class="offcanvas-content">
      <div class="close">
        <VBtn @click="closeMenu">
          <VIcon icon="$close" />
        </VBtn>
      </div>
      <h2
        class="text-center text-style"
        style="font-size: 3rem;"
      >
        {{ selectedItem?.dessert }}
      </h2>
      <div class="table-container">
        <p>Desserts (100g Servings):  <span>{{ selectedItem?.dessert }}</span></p>
        <p>Calories:  <span>{{ selectedItem?.calories }}</span></p>
        <p>Fat:  <span>{{ selectedItem?.fat }}</span></p>
        <p>Carbs:  <span>{{ selectedItem?.carbs }}</span></p>
        <p>Protein:  <span>{{ selectedItem?.protein }}</span></p>
      </div>
    </div>
    <div
      v-if="isOpen"
      class="offcanvas-backdrop"
      @click="closeMenu"
    />
  </div>
</template>

<style>
.offcanvas {
  position: fixed;
  z-index: 1000;
  background: white;
  block-size: 100%;
  box-shadow: -2px 0 5px rgba(0, 0, 0, 50%);
  inline-size: 50%;
  inset-block-start: 0;
  inset-inline-end: -50%; /* Adjust width as needed */
  transition: inset-inline-end 0.3s ease;
}

.offcanvas-open {
  inset-inline-end: 0;
}

.offcanvas-content {
  padding: 16px;
}

.offcanvas-backdrop {
  position: fixed;
  z-index: 999;
  background: rgba(0, 0, 0, 0%);
  block-size: 100%;
  inline-size: 100%;
  inset-block-start: 0;
  inset-inline-start: 0;
}

.close {
  display: flex;
  justify-content: flex-end;
  inline-size: 100%;
}

.table-container {
  overflow-x: auto;
}

.table-container p{
  color: black;
  font-size: 1.5rem;
  margin-block-start: 2%;
  margin-inline-start: 3%;
}

.table-container span{
  color: rgb(238, 33, 33);
}

@media (max-width: 768px) {
  .offcanvas {
    inline-size: 100%;
    inset-inline-end: -100%;
  }

  .offcanvas-open {
    inset-inline-end: 0;
  }

  .table-container {
    inline-size: 100%;
    overflow-x: auto;
  }

  thead th, tbody td {
    padding: 8px;
    font-size: 14px;
  }
}
</style>

<script setup>
 import { ref } from 'vue'
 import { nanoid } from 'nanoid'
 import { useStorage } from '@vueuse/core'
 import confetti from 'canvas-confetti'
 import { library } from '@fortawesome/fontawesome-svg-core'

 const newGrocery = ref('')
 const groceries = useStorage('groceries', [])

  const addGrocery = () => {
   if (newGrocery.value) {
    groceries.value.push({ id: nanoid(), name: newGrocery.value })
    newGrocery.value  = ''
   }
  }

  const deleteGrocery = (id) => {
    const removeIndex = groceries.value.findIndex(grocery => grocery.id === id)
    groceries.value.splice(removeIndex, 1)
    confetti({particleCount: 100,spread: 70,origin: { y: 0.6 }});
  }
</script>

<template>
  <main>
  <h1 class="title">Vue Grocerie List</h1> 
    <form class="newGroceryForm" @submit.prevent="addGrocery">
      <input id="newGrocery" autocomplete="off" type="text" placeholder="Add an item to your list" v-model="newGrocery" />
      <button type="submit"> <font-awesome-icon icon="fa-solid fa-cart-plus" /> </button>
    </form>
    <h3> Pending Items: {{groceries.length}}</h3>
    <ul>
      <li v-for="grocery in groceries" @click="deleteGrocery(grocery.id)">
        {{ grocery.name  }}
      </li>
    </ul>
  </main>
</template>

<style lang="postcss" scoped>
main {
  @apply mt-8 flex flex-col justify-center items-center gap-8;

  .title {
    @apply m-2 text-6xl font-extralight tracking-wider text-accent;
  }

  form {
    @apply flex focus-within:ring-8 focus-within:ring-accent focus-within:rounded-lg;
    input {
      @apply bg-white text-comment p-2 w-80 text-2xl rounded-l-md outline-none;
    }
    button {
      @apply bg-accent text-background p-2 text-2xl font-bold rounded-r-md;
      &:hover {
        @apply bg-purplish;
      }
    }
  }
  ul {
    @apply flex flex-col items-center justify-center rounded-lg bg-comment;
    li {
      @apply bg-white text-background m-2 p-2 w-96 text-center;
      &:hover {
        @apply bg-purplish font-bold cursor-pointer;
      }
    }
  }
}


</style>

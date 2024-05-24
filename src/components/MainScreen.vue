<template>
  <div class="screen">
    <h1>POKE MEMORIES</h1>
    <p>Select mode to start game</p>
    <div class="modes">
      <button v-for="item in itemMode" :key="item.itemBlock" @click="onStart(item.totalItemBlock)">
        <span>{{item.itemBlock}}x{{item.itemBlock}}</span>
        <span>{{item.mode}}</span>
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">

type Emit = (e: 'onStart', value: object) => void
interface ItemMode {
  itemBlock: number
  mode: string
  totalItemBlock: number
}
const itemMode = ref<ItemMode[]>([
  {
    itemBlock: 4,
    mode: 'Easy',
    totalItemBlock: 16
  },
  {
    itemBlock: 6,
    mode: 'Easy',
    totalItemBlock: 36
  },
  {
    itemBlock: 8,
    mode: 'Easy',
    totalItemBlock: 64
  },
  {
    itemBlock: 10,
    mode: 'Easy',
    totalItemBlock: 100
  },
])
const emit = defineEmits<Emit>()
const onStart = (totalOfBlock: number) => {
  emit("onStart", {totalOfBlock})
}
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: var(--dark);
  color: var(--light);
}

.screen h1 {
  font-size: 4.5rem;
  text-transform: uppercase;
}

.screen p {
  font-size: 2rem;
}

.modes {
  display: flex;
  margin-top: 2rem;
}

.modes button {
  font: var(--font);
  width: 150px;
  height: 150px;
  background: transparent;
  box-shadow: none;
  border: 1px solid var(--light);
  color: var(--light);
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 0 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}

.modes button:hover {
  background-color: var(--light);
  color: var(--dark);
}

.modes button span:first-child {
  font-size: 2rem;
}

.modes button span:last-child {
  display: block;
  font-size: 1.25rem;
  margin-top: 0.5rem;
}
</style>

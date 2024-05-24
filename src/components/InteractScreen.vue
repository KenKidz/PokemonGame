<template>
  <div class="screen">
    <div class="screen__inner" :style="{
      width: `${(((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 / 4 + 16) * Math.sqrt(cardsContext.length)}px`
    }">
      <Card v-for="(card, index) in cardsContext" :key="index" :img-back-face-url="`images/${card}.png`" ref="cardRef"
        :card="{ index, value: card }" :cardsContext="cardsContext" @on-flip="checkRule" />
    </div>
  </div>
</template>

<script setup lang="ts">
interface Prop {
  cardsContext: number[]
}
type Emit = (e: "onFinish") => void

const props = withDefaults(defineProps<Prop>(), {
  cardsContext: () => []
})
const emit = defineEmits<Emit>()

let rules = ref<any>([])
const instance: any = getCurrentInstance()
const cardRef = ref<any>([])
const { cardsContext } = toRefs(props)

const checkRule = (card: any) => {
  if (rules.value.length === 2) return false
  rules.value.push(card)
  if (rules.value.length === 2 && rules.value[0].value === rules.value[1].value) {
    cardRef.value[rules.value[0].index].onEnableDisableMode();
    cardRef.value[rules.value[1].index].onEnableDisableMode();
    rules.value = []
    const disabledElements = document.querySelectorAll(".screen .card.disabled")
    if (disabledElements && disabledElements.length === cardsContext.value.length - 2) {
      setTimeout(() => {
        emit("onFinish")
      }, 920)
    }
  } else if (rules.value.length === 2 && rules.value[0].value !== rules.value[1].value) {
    setTimeout(() => {
      if (instance) {
        cardRef.value[rules.value[0].index].onFlipBackCard();
        cardRef.value[rules.value[1].index].onFlipBackCard();
      }
      rules.value = []
    }, 800)
  } else {
    return false
  }
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
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;

}
</style>

<template>
  <div class="advice-container">
    <h1 class="advice__num" v-if="!isLoading">
      ADVICE #{{adviceNumber}}
    </h1>

    <AdviceText :adviceText="adviceText" :isLoading="isLoading" />

    <div class="divider-container">
      <!-- <svg v-if="isMobile" class="icon divider" width="295" height="16" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><path fill="#4F5D74" d="M0 8h122v1H0zM173 8h122v1H173z"/><g transform="translate(138)" fill="#CEE3E9"><rect width="6" height="16" rx="3"/><rect x="14" width="6" height="16" rx="3"/></g></g></svg>
      <svg v-else class="icon divider" width="444" height="16" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><path fill="#4F5D74" d="M0 8h196v1H0zM248 8h196v1H248z"/><g transform="translate(212)" fill="#CEE3E9"><rect width="6" height="16" rx="3"/><rect x="14" width="6" height="16" rx="3"/></g></g></svg> -->
      <img class="divider icon" :src='divider'  alt="divider">
    </div>

    <div @click='getAdvice' 
    :class="['dice-container', { 'pointered': !isLoading, 'no-pointer-event': isLoading}]"
    >
      <svg class="icon dice" width="24" height="24" xmlns="http://www.w3.org/2000/svg"><path d="M20 0H4a4.005 4.005 0 0 0-4 4v16a4.005 4.005 0 0 0 4 4h16a4.005 4.005 0 0 0 4-4V4a4.005 4.005 0 0 0-4-4ZM7.5 18a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm0-9a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm4.5 4.5a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm4.5 4.5a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm0-9a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Z" fill="#202733"/></svg>
    </div>
  </div>
</template>

<script setup>
import { ref, onBeforeMount, onUnmounted, onMounted, watch, watchEffect } from 'vue'
import debounce from 'debounce'
import AdviceText from './AdviceText.vue'
import { computed } from '@vue/reactivity';

// api url
const url = 'https://api.adviceslip.com/advice'
const size = { mobile: 'mobile', desktop: 'desktop'}
const dividerBaseURL = `../assets/pattern-divider-${size.mobile}.svg`


// states
const adviceNumber = ref()
const adviceText = ref('')
const isLoading = ref(true)
// const divider = ref(new URL(dividerBaseURL, import.meta.url))
const divider = ref('')
const isMobile = ref(true)

// functions
function changeWindowSize() {
  isMobile.value = window.innerWidth <= 800 ? true : false  
  divider.value = window.innerWidth > 800 ?
        new URL(`../assets/pattern-divider-${size.desktop}.svg`, import.meta.url): new URL(`../assets/pattern-divider-${size.mobile}.svg`, import.meta.url)
}

async function getAdvice() {
  isLoading.value = true
  const data = await (await fetch(url)).json()
  adviceNumber.value = data.slip.id
  adviceText.value = data.slip.advice
  isLoading.value = false
}

onMounted(async () => {
  changeWindowSize()
  await getAdvice()
})
  
onBeforeMount(() => {
  window.addEventListener('resize', debounce(changeWindowSize, 100))
})

onUnmounted(() => {
  window.removeEventListener('resize', changeWindowSize)
})

</script>



<style lang="scss" scoped>
// variables
$adviceContainerBg: hsl(217, 19%, 24%);
$adviceNum: hsl(150, 100%, 66%);

//mixins
%pointer { cursor: pointer; }

.pointered { 
  @extend %pointer;
  &:hover {
        box-shadow: 0 0 2rem $adviceNum;
      }
}

.no-pointer-event {
  pointer-events: none;
}


.advice-container {
  text-align: center;
  background-color: $adviceContainerBg;
  border-radius: .5rem;
  overflow-wrap: break-word;

  max-width: 30rem;
  padding: 2rem 1rem 0;
  margin: auto;
  // overflow: hidden;

  > .advice__num {
    color: $adviceNum;
    font-size: .6rem;
    letter-spacing: .12rem;
  }

  .icon  {
    display: block;
    // display: inline-block;
    margin: auto;
  }

  > .divider-container {
    // width: 100%;
    // overflow: hidden;
    display: table;
    margin-bottom: .5em;
    margin: auto;

    // @media (min-width: 800px) {
    //   > .divider {
    //     width: 100%;
    //   }
    // }

    > .divider {
        width: 100%;
        // width: 50%;
      }
  }

  > .dice-container {
      // @extend %pointer;
      background-color: $adviceNum;
      border-radius: 100%;
      margin: auto;
      padding: 1em;
      display: table;

      transform: translate(0, 50%);
  }
}

@media (min-width: 800px) {
  .advice-container {
    padding: 1.5rem 2.5rem 0;
  }
}
</style>
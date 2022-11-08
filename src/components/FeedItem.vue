<script setup>
import { reactive } from "@vue/runtime-core"

const state = reactive({buttonText: 'mehr', showContent: false})

function toggleContent() {
  state.showContent = !state.showContent
  state.buttonText = state.showContent ? 'weniger' : 'mehr'
}
</script>

<template>
  <div class="item">
    <h2>
      <slot name="heading"></slot>
    </h2>
    <p class="description"  >
      <slot name="description"></slot>
    </p>
    <p class="content" v-if="state.showContent">
      <slot></slot>
    </p>
    <button @click="toggleContent">{{ state.buttonText }}</button>
  </div>
</template>

<style scoped>
  .item {
    border: 1px solid #999;
    border-radius: 6px;
    padding: 0 8px 8px;
    margin-bottom: 16px;
    background-color: #EEE;
  }

  h2 {
    margin-bottom: 4px;
    font-weight: 500;
  }

  .description {
    margin-bottom: 20px
  }

  button {
    background: #2bafe5;
    color: #fff;
    border: none;
    border-radius: 3px;
    padding: 8px;
    width: 100%;
  }
</style>

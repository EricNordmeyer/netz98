<script setup>
import { reactive } from "@vue/runtime-core"

const props = defineProps({
  item: Object
})

const state = reactive({buttonText: 'mehr', showContent: false})

function toggleContent() {
  state.showContent = !state.showContent
  state.buttonText = state.showContent ? 'weniger' : 'mehr'
}
</script>

<template>
  <div class="item">
    <h2>
      <a :href="item.link">{{ item.title }}</a>
    </h2>
    <p class="description" v-html="item.description" />
    <p class="content" v-if="state.showContent" @click="toggleContent" v-html="item['content:encoded']" />
    <button @click="toggleContent">{{ state.buttonText }}</button>
  </div>
</template>

<style scoped>
  .item {
    border: 1px solid #ddd;
    border-radius: 6px;
    padding: 0 8px 8px;
    margin-bottom: 16px;
    background-color: #f6f6f6;
  }

  h2 {
    margin-bottom: 4px;
    font-weight: 500;
  }

  .description {
    margin-bottom: 20px;
  }
  .content {
    background: #fff;
    padding: 16px;
    border: 1px solid #ddd;
    border-radius: 6px;
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

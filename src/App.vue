<script setup>
import { onMounted, reactive } from "@vue/runtime-core"
import { XMLParser } from 'fast-xml-parser'
import FeedItem from './components/FeedItem.vue'

const state = reactive({ feedItems: [], title: '', description: '', channel: {} })

onMounted(() => {
  getRss()
})

function getRss() {
  try {
    const parser = new XMLParser();

    fetch('http://localhost:1234/feed/')
    .then(async (response) => {
      let result = await response.text();
      let rssFeed = parser.parse(result)
      state.channel = rssFeed.rss.channel
    })
  }
  catch {
    console.log("error")
  }
}
</script>

<template>
  <div>
    <header>
      <h1><a v-bind:href="state.channel.link" class="green"> {{ state.channel.title }}</a>{{ state.channel.description }}</h1>
    </header>
    <div v-for="item, index in state.channel.item" :key="index">
      <FeedItem>
        <template #heading><a :href="item.link">{{ item.title }}</a></template>
        <template #description><p v-html="item.description" /></template>
        <div v-html="item['content:encoded']" />
      </FeedItem>
    </div>

  </div>
</template>

<style scoped>
  header {
    margin-bottom: 24px;
  }

  h1 a{
    color: #009;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  }
</style>

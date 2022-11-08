<script setup>
import { onMounted, reactive } from "@vue/runtime-core"
import { XMLParser } from 'fast-xml-parser'
import FeedItem from './components/FeedItem.vue'
import HeaderItem from './components/HeaderItem.vue'


const state = reactive({ feedItems: [], title: '', description: '', channel: {} })

onMounted(() => {
  getRss()
})

function getRss() {
  const parser = new XMLParser();

  fetch('http://localhost:1234/feed/')
  .then((response) => {
    if (response.ok) {
      return response.text();
    }
  })
  .then((result) => {
      let rssFeed = parser.parse(result)
      state.channel = rssFeed.rss.channel
    }
  )
  .catch((error) => {
    console.log(error)
  })
}

</script>

<template>
  <div>
    <HeaderItem :homeLink="state.channel.link">
      <template #brand>{{ state.channel.title }}</template>
      <template #description>{{ state.channel.description }}</template>
    </HeaderItem>
    <div v-for="item, index in state.channel.item" :key="index">
      <FeedItem :item="item" />
    </div>

  </div>
</template>

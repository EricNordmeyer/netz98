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
    <HeaderItem :homeLink="state.channel.link">
      <template #brand>{{ state.channel.title }}</template>
      <template #description>{{ state.channel.description }}</template>
    </HeaderItem>
    <div v-for="item, index in state.channel.item" :key="index">
      <FeedItem>
        <template #heading><a :href="item.link">{{ item.title }}</a></template>
        <template #description><p v-html="item.description" /></template>
        <div v-html="item['content:encoded']" />
      </FeedItem>
    </div>

  </div>
</template>

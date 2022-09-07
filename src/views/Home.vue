<script setup>
import { store } from '../store'
import supabase from '../supabase'

const getWordsNumber = (str) => (str.split(' ').length)

const fetchPosts = async () => {
  let { data: posts, error } = await supabase
  .from('posts')
  .select()

  if (error) throw new Error(error)

  store.posts = posts
} 

fetchPosts()
</script>

<template>
  <div class="Home">
    <main class="container mx-auto">
      <div v-if="store.posts.length < 1">
        There is no posts.
      </div>
      <div v-else>
        <div class="PostItem border border-slate-200 mb-4 p-4 rounded-lg cursor-pointer" v-for="item, itemIndex in store.posts" :key="itemIndex" @click="$router.push(`/post/${item.id}`)">
          <h1 class="text-slate-900 text-3xl font-bold">
            {{ item.title }}
          </h1>
          <p>{{ getWordsNumber(item.description) }} words.</p>
        </div>
      </div>
    </main>
  </div>
</template>
<script setup>
import { reactive } from 'vue'
import { useRoute } from 'vue-router'
import {store} from '../store'
import supabase from '../supabase'

const route = useRoute()

let post = reactive({})

const fetchPost = async (id) => {
  const found = store.posts.find(x => x.id === parseInt(route.params.id))
  if (found) {
    return
  }
  let { data, error } = await supabase
  .from('posts')
  .select()
  .eq('id', id)
  .single()

  if (error) throw new Error(error)

  Object.assign(post, data)
} 

fetchPost(route.params.id)
</script>

<template>
  <div class="Post text-center container mx-auto">
    <div v-if="!post">
      No post found.
    </div>
    <div v-else>
      <h1 class="text-slate-900 text-3xl font-bold mb-4">{{ post.title }}</h1>
      <p class="text-md text-slate-300">{{ post.created_at }}</p>
      <p class="text-xl text-slate-500">{{ post.description}}</p>
    </div>
  </div>
</template>
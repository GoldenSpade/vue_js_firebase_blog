<template>
  <h1>Real-time data</h1>
  <div v-for="post in posts" :key="post.id">{{ post.title }}</div>
</template>
<script>
import {ref} from 'vue'
import {projectFirestore} from '@/firebase/config'
export default {
  setup() {
    const posts = ref([])

    projectFirestore
      .collection('posts')
      .orderBy('createAt', 'desc')
      .onSnapshot((snap) => {
        let docs = snap.docs.map((doc) => {
          return {...doc.data(), id: doc.id}
        })
        posts.value = docs
      })

    return {
      posts,
    }
  },
}
</script>
<style>
</style>
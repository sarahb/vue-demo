<template>
   <div class="container">
     <div class="title" v-if="loaded">
       <h1>{{user.name}}</h1>
       <hr>
       <h3>{{user.email}} | {{user.address.city}}</h3>
     </div>
     <Blog v-if="loaded" v-for="article in articles"
        :article="article"
        :key="article.id"
     />
   </div>
</template>

<script>
import { Blog } from '@/components'

export default {
  name: 'Profile',
  props: {
    id: Number
  },
  data() {
     return {
        user: {},
        articles: [],
        loaded: false
     }
  },
  components: {
    Blog
  },
  methods: {},

  beforeMount() {
    Promise.all([
      fetch('https://jsonplaceholder.typicode.com/users/'+this.id)
        .then(res => res.json())
        .then(data => {this.user = data}),
      fetch('https://jsonplaceholder.typicode.com/posts/?userId='+this.id)
        .then(res => res.json())
        .then(data => {this.articles = data})
    ])
    .then(() => {
       // jesus (christ)
       this.articles = this.articles.map(item => {
          item = Object.assign({},item,{ author: this.user })
          return item
       }).sort((a, b) => a.title.length - b.title.length)
    })
    .then(() => {
      this.loaded = !this.loaded
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
   margin: auto;
}
.title {
  text-align: center;
  margin: 0px 20px 20px 20px;
}
h1 {
  margin-top: 1em;
}
h3 {
  margin-top: 1em;
  font-size: 0.9em;
  color: #777777;
}
hr {
  margin-top: 1em;
  color: black;
  border: none;
  height: 2px;
  width: 100%;
  background-color: #325D88;;
}
</style>

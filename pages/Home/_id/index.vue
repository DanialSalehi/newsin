<template>
  <v-row justify="center" align="center">
    <v-col>
      <v-card v-for="post in pagePosts" :key="post.id" class="my-3" hover>
        <v-card-title>{{ post.title }}</v-card-title>
        <v-card-text>{{ post.body }}</v-card-text>
        <v-card-subtitle>{{ post.id }}</v-card-subtitle>
      </v-card>
      <v-pagination
        v-model="page"
        :length="posts.length / 10"
        circle
      ></v-pagination>
      <br />
      <br />
      <br />
      <v-btn small elevation="4" color="primary">click</v-btn>
    </v-col>
  </v-row>
</template>

<script>
export default {
  created() {
    var p = Number(this.$route.params.id)
    console.log(p)
    this.page = p
    p *= 10
    this.pagePosts = this.posts.slice(p - 10, p)
  },
  components: {},
  async asyncData({ $axios }) {
    const posts = await $axios.$get(
      'https://jsonplaceholder.typicode.com/posts'
    )
    var pagePosts = posts.slice(0, 10)
    return {
      posts,
      pagePosts,
      page: 1,
    }
  },
  watch: {
    page: function (val, oldVal) {
      this.$router.push('/Home/' + val)
    },
  },
}
</script>

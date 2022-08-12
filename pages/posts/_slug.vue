<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-lg-8">
          <nav aria-label="breadcrumb" class="mt-4">
            <ol class="breadcrumb">
              <li class="breadcrumb-item"><nuxt-link to="/">Главная</nuxt-link></li>
              <li class="breadcrumb-item active" aria-current="page">{{ post.h1 }}</li>
            </ol>
          </nav>
          <img class="img-fluid rounded " :src="post.image" alt="">
          <hr>
          <h4 class="card-title">{{ post.h1 }}</h4>
          <p v-html="post.text"></p>
          <div class="d-flex justify-content-end">
            <span v-for="tag in post.tags">
                  <nuxt-link :to="`/tags/${tag}`" class="mr-1 badge badge-info">#{{ tag }}</nuxt-link>
            </span>
          </div>
          <hr>
          <div class="d-flex">
            <div class="mr-auto p-2 lead">Автор: {{ post.author }}</div>
            <div class="p-2">Опубликовано: {{ post.post_date }}</div>
          </div>
          <hr>
          <Comments :comments="comments"/>
        </div>
        <Aside :tags=tags :aside=aside />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import post_detail from "@/layouts/post_detail";
import Aside from "@/components/Aside";
import Comments from "@/components/Comments";
export default {
  components: {
    Aside,
    Comments
  },
  layout: "post_detail",
  async asyncData({params}) {
    console.log(params)
    const post = await axios.get(`http://127.0.0.1:8000/blogapi/posts/${params.slug}`);
    const tags = await axios.get(`http://127.0.0.1:8000/blogapi/tags/`);
    const aside = await axios.get(`http://127.0.0.1:8000/blogapi/aside/`);
    const comments = await axios.get(`http://127.0.0.1:8000/blogapi/comments/${params.slug}`);
    return {
      post: post.data,
      tags: tags.data,
      aside: aside.data,
      comments: comments.data
    }
  },
}
</script>

<style scoped>

</style>
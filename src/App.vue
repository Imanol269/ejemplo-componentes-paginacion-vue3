<script setup>
import { ref, onMounted } from "vue";
import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";

const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(true);

const favorito = ref("");
const cambiarFavorito = (title) => {
  favorito.value = title;
};

const next = () => {
  inicio.value = inicio.value + postXpage;
  fin.value = fin.value + postXpage;
};

const prev = () => {
  inicio.value = inicio.value - postXpage;
  fin.value = fin.value - postXpage;
};

onMounted(async () => {
  loading.value = true;
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    connsole.log(error);
  } finally {
    setTimeout(() => {
      loading.value = false;
    }, 2000);
  }
});

//ERTA ES LA MEJOR FORMA DE HACERLO

// fetch("https://jsonplaceholder.typicode.com/posts")
//   .then((res) => res.json())
//   .then((data) => {
//     posts.value = data;
//   })
//   .finally(() => {
//     setTimeout(() => {
//       loading.value = false;
//     }, 2000);
//   });
</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <h1>App</h1>
    <h2>Mi post favorito: {{ favorito }}</h2>

    <PaginatePost
      :fin="fin"
      :inicio="inicio"
      @next="next"
      @prev="prev"
      class="mb-2"
      :maxlength="posts.length"
    />

    <BlogPost
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :title="post.title"
      :id="post.id"
      :body="post.body"
      class="mb-2"
      @cambiarFavoritoNombre="cambiarFavorito"
    ></BlogPost>
  </div>
</template>

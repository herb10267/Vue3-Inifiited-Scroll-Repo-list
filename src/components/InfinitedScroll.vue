<template lang="">
  <div class="container">
    <h1>Repo list</h1>
    <h2>https://github.com/vuejs</h2>

    <div class="card" v-for="repo in repos" :key="repo.html_url">
      <div class="card-header">{{ repo.name }}</div>
      <div class="card-body">
        <blockquote class="blockquote mb-0">
          <p>{{ repo.description }}</p>
          <footer class="blockquote-footer">
            <cite title="Source Title">{{ repo.html_url }}</cite>
          </footer>
        </blockquote>
      </div>
    </div>
    <div class="spinner-border" role="status" v-if="isLoading">
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>
</template>
<script>
import { ref, onMounted } from "vue";
import axios from "axios";
// import repoData from "../data/repo.js";

export default {
  name: "componentD",
  setup() {
    const repos = ref([]);
    const row = ref(6);
    const page = ref(0);
    const isLoading = ref(false);

    // methods
    // const loadRepo = () => {
    //     isLoading.value = true;
    //     setTimeout(()=>{
    //         repoData.forEach(element => {
    //             repos.value.push(element);
    //         });
    //         isLoading.value = false;
    //         page.value++;
    //     }, 1000);
    // }

    const loadRepo = () => {
      page.value += 1;
      isLoading.value = true;
      const url = `https://api.github.com/users/vuejs/repos?per_page=${row.value}&page=${page.value}`;
      axios.get(url).then((response) => {
        // handle success
        response.data.forEach((element) => {
          repos.value.push(element);
        });
        isLoading.value = false;
      });
    };

    const handleScroll = () => {
      if (
        window.innerHeight + window.scrollY >=
        document.body.offsetHeight * 0.7
      ) {
        if (isLoading.value) return;
        loadRepo();
      }
    };

    // init
    loadRepo();

    // addEventListener
    onMounted(() => {
      window.addEventListener("scroll", handleScroll);
    });

    return {
      repos,
      row,
      page,
      isLoading,
    };
  },
};
</script>
<style>
.card {
  margin-top: 20px;
}
</style>

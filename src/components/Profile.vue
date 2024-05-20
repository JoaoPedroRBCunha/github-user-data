<script setup>
import { reactive, ref, computed, onMounted, onUpdated, onUnmounted } from 'vue';

const searchInput = ref('')

const state = reactive({
    name: '',
    login: '',
    bio: '',
    company: '',
    avatar_url: '',
    repos: []
});

async function fetchGithubUser(ev) {
    ev.preventDefault()
    try {
    const res = await fetch(`https://api.github.com/users/${searchInput.value}`);
    if (!res.ok) {
            if (res.status === 404) {
                console.log("Usuário não encontrado");
                return; // Sai da função se o usuário não for encontrado
            }
            throw new Error(`Erro ao buscar usuário: ${res.status}`); // Lança um erro para outros códigos de status
        }
    const { login, name, bio, company, avatar_url } = await res.json();

    state.login = login;
    state.name = name;
    state.bio = bio;
    state.company = company;
    state.avatar_url = avatar_url;

    fetchUserRepositories();
    
    } catch (error){
        console.log("Não foi possível achar usuário")
    }
}

async function fetchUserRepositories() {
    const res = await fetch(`https://api.github.com/users/${state.login}/repos`)
    const repos = await res.json();

    state.repos = repos;
};

const repoCountMessage = computed(() => {
    return state.repos.length > 0 ? 
        `${state.name} possui ${state.repos.length} repositórios públicos \n Repositórios:`
        : `${state.name} não possui nenhum repositórios público` 
});

onMounted(() => {
    console.log("O componente foi montado")
})

onUpdated(() => {
    console.log("O componente foi atualizado")
})

onUnmounted(() =>{
    console.log("O componente foi desmontado")
})
</script>

<template>
    <h1>GitHub User Data</h1>
    <form @submit="fetchGithubUser">
        <input type="text" v-model.lazy="searchInput">
        <button>Carregar Usuário</button>
    </form>
    <!-- @ == v-on: -->
    <div v-if="state.login !== ''">
        <img :src="state.avatar_url">
        <!-- : == v-bind -->
        <strong>@{{state.login}} </strong> 
        <h1> {{state.name}} </h1>
        <h2> {{state.company}} </h2>
        <span> {{state.bio}} </span>
        <h2> {{ repoCountMessage }}</h2>
    </div>

    <section v-if="state.repos.length > 0">
        <article v-for="repo of state.repos">
            <h3>{{ repo.full_name }}</h3>
            <p>{{ repo.description }}</p>
            <a :href="repo.html_url" target="_blank">Ver no GitHub</a>
        </article>
    </section>
</template>

<style scoped>
img {
    border: 1px solid #e5e5e5;
    border-radius: 999px;
    display: block;
    margin: 1rem auto;
    width: 8rem;
    height: 8rem;
  }
  
  h1, h2 {
    color: #f64348;
    margin: 1rem auto .25rem;
  }
  
  h3 {
    margin: 1rem auto .25rem;
  }
  
  span{
    display: block;
    margin: 1rem auto;
  }
  
  input,
  button {
    max-width: 20rem;
    padding: .5rem;
  }
  
  input {
    background-color: #1c1a1d;
    border: 1px solid #f64348;
    border-radius: .25rem;
    color: #e5e5e5;
    margin: 1rem 1rem 1rem 0;
  }
  
  button {
    background-color: #f64348;
    border: unset;
    border-radius: .25rem;
    color: #1c1a1d;
    font-size: 1rem;
    font-weight: 700;
    text-transform: uppercase;
  }
  
  button:hover {
    cursor: pointer;
    filter: brightness(0.95);
  }
  
  a {
    color: #f64348;
  }
</style>
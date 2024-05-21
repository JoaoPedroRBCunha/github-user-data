<script setup>
    import { ref } from 'vue'
    import Profile from '../components/Profile.vue'

    const isOpen = ref(true)
    function isOpenOrClose() {
        return isOpen.value ? 'Habilitado - clique na caixa para desabilitar' : 'Desabilitado - clique na caixa para habilitar'
    }
</script>

<template>
    <input type="checkbox" v-model="isOpen"> {{ isOpenOrClose() }}
    <Profile v-if="isOpen">
    <h1>GitHub User Data</h1>

    <template v-slot:footer>
      <footer> 
        Desenvolvido por <a href="https://github.com/JoaoPedroRBCunha" target="_blank"> JoaoPedroRBCunha</a>
      </footer>
    </template>
    </Profile>
</template>
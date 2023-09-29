<template>
    <header>
        <HeaderView :toggleModal="toggleModal" />
    </header>
    
    <main>
        <Admin :toggleModal="toggleModal" />
        <NuxtPage :rawPosts="rawPosts" :refresh="refresh" />
    </main>

    <footer>
        <FooterView />
    </footer>
</template>

<script setup>
import { url } from "~/api/index";

const toggleModal = (id) => {
    const element = document.getElementById(id);

    if(element.classList.contains("hidden")) {
        element.classList.remove("hidden");
    } else {
        element.classList.add("hidden");
    }
}

const { data: rawPosts, refresh } = await useFetch(url + "posts");
</script>

<style lang="scss" scoped>

</style>
<template>
    <header>
        <HeaderView :toggleModal="toggleModal" />
    </header>

    <main>
        <ClientOnly>
            <Admin :toggleModal="toggleModal" />
            <NuxtPage :rawPosts="rawPosts" :refresh="refresh" />
        </ClientOnly>
    </main>

    <footer>
        <FooterView />
    </footer>
</template>

<script setup>
import { url } from "~/api/index";

const toggleModal = (id) => {
    const element = document.getElementById(id);

    if (element.classList.contains("hidden")) {
        element.classList.remove("hidden");
    } else {
        element.classList.add("hidden");
    }
}

const rawPosts = ref([]);

const { data, refresh } = await useFetch(url + "posts");

rawPosts.value = data.value.reverse();
</script>

<style lang="scss" scoped></style>
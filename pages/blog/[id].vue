<template>
    <Head>
        <Title>Blog</Title>
    </Head>
    <div class="my-[200px]">
        <ul
            class="mx-auto w-[75%] text-center grid lg:grid-cols-3 md:grid-cols-2 sm:grid-cols-1 max-sm:grid-cols-1 gap-3">
            <li class="border border-gray-400 bg-white shadow-lg shadow-gray-400 p-3 aspect-square" v-for="post in posts"
                :key="post.id">
                <img :src="post.poster" alt="" class="w-[100%] aspect-square">
                <p class="mt-1">{{ post.title }}</p>
            </li>
        </ul>

        <ul class="mt-[50px] w-[170px] mx-auto text-center grid grid-cols-3 gap-2">
            <li class="cursor-pointer aspect-square rounded-full p-2 border border-gray-400 grid justify-center items-center">
                <NuxtLink :to="parseInt(pageNumber) - 1 == 0 ? '' : `/blog/${parseInt(pageNumber) - 1}`">{{ parseInt(pageNumber) - 1 == 0 ? "-" : parseInt(pageNumber) - 1 }}</NuxtLink>
            </li>
            <li class="cursor-pointer aspect-square rounded-full p-2 border border-gray-400 grid justify-center items-center">
                {{ pageNumber }}
            </li>
            <li class="cursor-pointer aspect-square rounded-full p-2 border border-gray-400 grid justify-center items-center">
                <NuxtLink :to="parseInt((3*parseInt(pageNumber)) >= rawPosts.length) ? '' : `/blog/${parseInt(pageNumber) + 1}`">{{ parseInt((3*parseInt(pageNumber))) >= rawPosts.length ? "-" : parseInt(pageNumber) + 1 }}</NuxtLink>
            </li>
        </ul>
    </div>
</template>

<script setup>
import { url } from "~~/api"

const posts = ref([]);
const { data: rawPosts, refresh } = await useFetch(url + "posts");

const { id: pageNumber } = useRoute().params;

if(rawPosts.length < 4) {
    posts.value = rawPosts.value;
} else {
    posts.value = rawPosts.value;
    posts.value = posts.value.reverse();
    posts.value = posts.value.slice((3*parseInt(pageNumber)) - 3, (3*parseInt(pageNumber)));
}

</script>

<style lang="scss" scoped></style>
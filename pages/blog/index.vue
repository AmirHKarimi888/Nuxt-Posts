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
                <p class="w-[50%] mx-auto p-2 rounded-lg bg-cyan-700 text-white my-1">
                    <NuxtLink :to="`/blog/posts/${post.id}`">Go To Page</NuxtLink>
                </p>
            </li>
        </ul>

        <ul class="mt-[50px] w-[170px] mx-auto text-center grid grid-cols-3 gap-2">
            <li class="cursor-pointer aspect-square rounded-full p-2 border border-gray-400 grid justify-center items-center">
                -
            </li>
            <li class="cursor-pointer aspect-square rounded-full p-2 border border-gray-400 grid justify-center items-center">
                1
            </li>
            <li class="cursor-pointer aspect-square rounded-full p-2 border border-gray-400 grid justify-center items-center">
                <NuxtLink to="/blog/2">2</NuxtLink>
            </li>
        </ul>
    </div>
</template>

<script setup>
import { url } from "~~/api"
const posts = ref([]);
const { data: rawPosts, refresh } = await useFetch(url + "posts");
if(rawPosts.length < 7) {
    posts.value = rawPosts.value;
} else {
    posts.value = rawPosts.value;
    posts.value = posts.value.slice(posts.value.length - 3, posts.value.length);
}
</script>

<style lang="scss" scoped></style>
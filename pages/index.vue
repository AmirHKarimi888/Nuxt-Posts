<template>
    <div>
        <ul
            class="mx-auto my-[200px] w-[75%] text-center grid lg:grid-cols-3 md:grid-cols-2 sm:grid-cols-1 max-sm:grid-cols-1 gap-3">
            <li class="border border-gray-400 bg-white shadow-lg shadow-gray-400 p-3 aspect-square" v-for="post in posts"
                :key="post?.id">
                <img :src="post?.poster" alt="" class="w-[100%] aspect-square">
                <p class="mt-1">{{ post?.title }}</p>
                <p class="w-[50%] cursor-pointer mx-auto p-2 rounded-lg bg-cyan-700 text-white my-1">
                    <NuxtLink :to="`/blog/posts/${post.id}`">Go To Page</NuxtLink>
                </p>
            </li>
        </ul>
    </div>
</template>

<script setup>
import { url } from "~/api";

const posts = ref([]);
const { data: rawPosts, refresh } = await useFetch(url + "posts");
if (rawPosts.length < 7) {
    posts.value = rawPosts.value;
} else {
    posts.value = rawPosts.value;
    posts.value = posts.value.slice(posts.value.length - 6, posts.value.length);
    posts.value = posts.value.reverse();
}
</script>

<style lang="scss" scoped></style>
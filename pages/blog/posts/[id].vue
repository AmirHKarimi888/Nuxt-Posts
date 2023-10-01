<template>
    <Head>
        <Title>{{ post.title }}</Title>
    </Head>
    <div>
        <div class="my-[200px] mx-auto rounded-sm lg:w-[60%] md:w-[75%] sm:w-[84%] max-sm:w-[90%] border border-gray-400 shadow-lg shadow-gray-300 p-8">
            <div class="header mx-auto p-3 bg-gray-200 grid grid-cols-2">
                <p class="text-center"><i class="fa fa-heart mx-1 mt-1"></i>{{ post.likedBy.length }}</p>
                <p class="text-center"><i class="fa fa-eye mx-1 mt-1"></i>{{ post.views }}</p>
            </div>
            <img :src="post.poster" class="mx-auto mt-5 w-[100%] aspect-video" alt="">
            <p class="mt-5 mx-auto text-[140%] text-center">{{ post.title }}</p>
            <p class="mt-5 mx-auto text-[100%] text-left">{{ post.description }}</p>
            <button class="mt-5 text-[156%]" @click="like">
                <i :class="post?.likedBy.includes(loggedInUser) ? 'fa fa-heart text-red-500' : 'fa fa-heart-o'"></i>
            </button>
        </div>       
    </div>
</template>

<script setup>
import { url } from '~/api';

const { id: postId } = useRoute().params;
const loggedInUser = localStorage.getItem('loggedInUser');

const { data: post, pending, refresh } = await useFetch(url + "posts/" + postId);

onMounted(async() => {
    await useFetch(url + "posts/" + postId, {
            method: "put",
            body: {
                ...post.value,
                views: parseInt(post.value.views) + 1
            }
        }).then(() => {
            refresh();
        })
})

const like = async() => {
    if(post.value.likedBy.includes(loggedInUser)) {
        await useFetch(url + "posts/" + postId, {
            method: "put",
            body: {
                ...post.value,
                likedBy: post.value.likedBy.filter((item) => {
                    if(item != loggedInUser) {
                        return item;
                    }
                })
            }
        }).then(() => {
            refresh();
        })
    } else {
        await useFetch(url + "posts/" + postId, {
            method: "put",
            body: {
                ...post.value,
                likedBy: [...post.value.likedBy, loggedInUser]
            }
        }).then(() => {
            refresh();
        })
    }
}
</script>

<style lang="scss" scoped>

</style>
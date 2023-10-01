<template>
    <div @click.self="toggleModal('adminModal')" id="adminModal" class="admin w-full h-screen fixed top-0 left-0 backdrop-blur-sm overflow-y-scroll hidden">
        <div v-if="checkPassword !== password ? true : false"
            class="mx-auto text-center lg:w-[70%] md:w-[75%] sm:w-[81%] max-sm:w-[90%] mt-[200px] p-6 border border-gray-400 bg-gray-200 rounded-lg">
            <input type="text" v-model="checkPassword" class="border border-gray-400 rounded-lg p-1 my-1"
                placeholder="Enter The Password">
        </div>

        <div v-else
            class="mx-auto text-center lg:w-[70%] md:w-[75%] sm:w-[81%] max-sm:w-[90%] my-[200px] p-6 border border-gray-400 bg-gray-200 rounded-lg">
            <input type="text" v-model="title" class="border border-gray-400 rounded-lg p-1 my-1" placeholder="Title"><br>
            <input type="text" v-model="poster" class="border border-gray-400 rounded-lg p-1 my-1" placeholder="Poster"><br>
            <input type="text" v-model="descr" class="border border-gray-400 rounded-lg p-1 my-1"
                placeholder="Description"><br>
            <button @click="selectedPost == '' ? createPost() : editPost()" class="my-1 bg-cyan-600 p-2 rounded-lg text-white">{{ selectedPost == "" ? "Post" : "Edit" }}</button><br><br>

            <ul
                class="mx-auto w-[90%] mt-5 text-center grid lg:grid-cols-3 md:grid-cols-2 sm:grid-cols-1 max-sm:grid-cols-1 gap-3">
                <li class="border border-gray-400 bg-white shadow-lg shadow-gray-400 p-3 aspect-square"
                    v-for="post in posts" :key="post.id">
                    <img :src="post.poster" alt="" class="w-[100%] aspect-square">
                    <p class="mt-1">{{ post.title }}</p>
                    <button @click="deletePost(post)" class="bg-red-500 text-white rounded-lg text-xs p-2 mt-2 mx-1">Delete</button>
                    <button @click="startEditing(post)" class="text-white rounded-lg text-xs p-2 mt-2 mx-1"
                        :class="selectedPost != post ? 'bg-gray-600' : 'bg-green-500'">Edit</button>
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { url } from "~/api/index";

const { toggleModal } = defineProps(["toggleModal"]);

const password = "123";
const checkPassword = ref("");

const posts = ref([]);

const title = ref("");
const poster = ref("");
const descr = ref("");

const { data, status, refresh } = await useFetch(url + "posts/");

posts.value = data.value;

const createPost = () => {
    let latestId = 0;
    for (let post of posts.value) {
        if (post) {
            if (parseInt(post?.id) > parseInt(latestId)) {
                latestId = parseInt(post.id);
            }
        } else {
            latestId = 0;
        }
    }

    let date = new Date();

    useFetch(url + "posts", {
        method: "post",
        body: {
            id: latestId + 1,
            created: `${date.getFullYear()}/${date.getMonth() + 1}/${date.getDate()} At ${date.getHours()}:${date.getMinutes()}`,
            updated: "",
            title: title.value,
            poster: poster.value,
            views: 0,
            description: descr.value,
            likedBy: [],
            comments: []
        }
    })
        .then(() => {
            title.value = "";
            poster.value = "";
            descr.value = "";
        })
        .then(() => {
            refresh()
                .then(() => {
                    posts.value = data.value;
                })
        })
}

const selectedPost = ref("");

const startEditing = (post) => {
    if (selectedPost.value == "") {
        selectedPost.value = post;

        title.value = post.title;
        poster.value = post.poster;
        descr.value = post.description;
    } else if(selectedPost.value == post) {
        selectedPost.value = "";

        title.value = "";
        poster.value = "";
        descr.value = "";
    }
}

const editPost = async() => {
    let date = new Date();

    await useFetch(url + "posts/" + selectedPost.value.id, {
        method: "put",
        body: {
            ...selectedPost.value,
            updated: `${date.getFullYear()}/${date.getMonth() + 1}/${date.getDate()} At ${date.getHours()}:${date.getMinutes()}`,
            title: title.value,
            poster: poster.value,
            description: descr.value,
        }
    })
        .then(() => {
            selectedPost.value = "";

            title.value = "";
            poster.value = "";
            descr.value = "";
        })
        .then(() => {
            refresh()
                .then(() => {
                    posts.value = data.value;
                })
        })
}

const deletePost = async(post) => {
    await useFetch(url + "posts/" + post.id, {
        method: "delete"
    })
    .then(() => {
            refresh()
                .then(() => {
                    posts.value = data.value;
                })
        })
}
</script>

<style lang="scss" scoped></style>
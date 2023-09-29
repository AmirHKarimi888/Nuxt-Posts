<template>
    <nav class="w-full p-2 border-b border-gray-400 grid justify-center items-center">
        <ul class="flex gap-3">
            <li class="cursor-pointer p-2">
                <NuxtLink to="/">Home</NuxtLink>
            </li>
            <li class="cursor-pointer p-2">
                <NuxtLink to="/blog">Blog</NuxtLink>
            </li>
            <li @click="toggleModal('adminModal')" class="cursor-pointer bg-cyan-600 text-center text-white rounded-lg p-2">
                Admin
            </li>
        </ul>
    </nav>
</template>

<script setup>
const { toggleModal } = defineProps(["toggleModal"]);
</script>

<style lang="scss" scoped>

</style>
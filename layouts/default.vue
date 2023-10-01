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

if("loggedInUser" in localStorage) {
    if(localStorage.getItem("loggedInUser") == "") {
        localStorage.setItem("loggedInUser", Math.round(Math.random() * 10000000000));    
    }
} else {
    localStorage.setItem("loggedInUser", Math.round(Math.random() * 10000000000));
}

const toggleModal = (id) => {
    const element = document.getElementById(id);

    if (element.classList.contains("hidden")) {
        element.classList.remove("hidden");
    } else {
        element.classList.add("hidden");
    }
}

</script>

<style lang="scss" scoped></style>
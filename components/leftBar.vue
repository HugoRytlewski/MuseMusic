<script setup>
    const router = useRoute()
    console.log(router.name);
    
</script>

<template>

    <div class="select-none fixed hidden md:flex h-[52rem] w-20  border-neutral-700  flex-col items-center justify-center gap-20 pl-10">
        <div class="flex flex-col gap-3 bg-neutral-900 items-center justify-center rounded-full p-2 ">

        <div class="h-14 w-14 rounded-full hover:bg-neutral-800 duration-300 flex items-center justify-center cursor-pointer p-2"             :class="router.name === 'index' ? 'bg-neutral-800' : 'bg-none'">
            <img 
            src="../assets/icon/home.svg" 
            alt="" 
            class="h-5 "
            >
        </div>
        <div class="h-14 w-14 rounded-full hover:bg-neutral-800 duration-300 flex items-center justify-center cursor-pointer">
            <img src="../assets/icon/search.svg" alt="" class="h-5   cursor-pointer">

        </div>
        <div class="h-14 w-14 rounded-full hover:bg-neutral-800 duration-300 flex items-center justify-center cursor-pointer">
            <img src="../assets/icon/favorite.svg" alt="" class="h-5  cursor-pointer">

        </div>
        <div class="h-14 w-14 rounded-full hover:bg-neutral-800 duration-300 flex items-center justify-center cursor-pointer">
            <img src="../assets/icon/library.svg" alt="" class="h-5  cursor-pointer">

        </div>
    </div>

    </div>
</template>
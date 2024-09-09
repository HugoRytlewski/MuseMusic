<script setup>
    const router = useRoute()
    
</script>

<template>

    <div class="select-none fixed z-50 flex md:h-[52rem] md:w-20 w-screen bottom-0   border-neutral-300  md:flex-col items-center justify-center gap-20 md:pl-10">
        <div class="flex md:flex-col gap-3 w-full md:w-fit bg-neutral-900 bg-opacity-80  backdrop-blur-2xl items-center justify-center md:rounded-full rounded-t-3xl p-2 ">
            <nuxt-link to="/">
                <div 
                class="h-14 w-14 rounded-full hover:bg-neutral-800 duration-300 flex items-center justify-center cursor-pointer p-2" 
                :class="router.name === 'index' ? 'bg-neutral-800' : 'bg-none'"
                >
                    <img src="../assets/icon/home.svg" alt="" class="h-5 ">
                </div>
            </nuxt-link>

            <nuxt-link to="/search">
                <div 
                class="h-14 w-14 rounded-full hover:bg-neutral-800 duration-300 flex items-center justify-center cursor-pointer"
                :class="router.name === 'search' ? 'bg-neutral-800' : 'bg-none'"
                
                >
                    <img src="../assets/icon/search.svg" alt="" class="h-5   cursor-pointer">
                </div>
            </nuxt-link>

            <nuxt-link to="/favorite">
                <div 
                class="h-14 w-14 rounded-full hover:bg-neutral-800 duration-300 flex items-center justify-center cursor-pointer"
                :class="router.name === 'favorite' ? 'bg-neutral-800' : 'bg-none'"
                >
                    <img src="../assets/icon/favorite.svg" alt="" class="h-5  cursor-pointer">
                </div>
            </nuxt-link>

            <div class="h-14 w-14 rounded-full hover:bg-neutral-800 duration-300 flex items-center justify-center cursor-pointer">
                <img src="../assets/icon/library.svg" alt="" class="h-5  cursor-pointer">
            </div>

        </div>
    </div>
</template>
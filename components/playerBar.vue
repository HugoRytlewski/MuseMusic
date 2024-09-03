<script setup>
    const isPlaying = ref(false);
    const musicPosition = ref(0);
    const isOpen = ref(false);

    function play() {
        isPlaying.value = true;
    }
    function stop() {
        isPlaying.value = false;
    }
    function next() {        
        if (musicPosition.value === music.length - 1) {
            musicPosition.value = 0;
        } else
        musicPosition.value++;
    }
    
    function previous() {
        if (musicPosition.value === 0) {
            musicPosition.value = music.length - 1;
        } else
        musicPosition.value--;
    }
    
    const music = (
        [
            {
                title: "Destin",
                artist: "Ninho",
                duration: "03:04",
                cover: "/img/ni.jpg",
                src: "../assets/music/bella.mp3"
            },
            {
                title: "EuroStar",
                artist: "Ninho",
                duration: "03:04",
                cover: "/img/ni2.jpg",
                src: "../assets/music/sapes_comme_jamais.mp3"
            },
        ]
    );
    
</script>

<template>
    <div class="fixed bottom-0 h-20 w-screen  flex items-end justify-center group ">
        <div  class="fixed  bottom-0  h-[3.5rem] md:h-20 md:w-[20rem] md:group-hover:w-[35rem] overflow-hidden w-[90vw]  duration-300 mb-3 md:mb-6  rounded-full md:pl-10 pr-10 bg-neutral-900 flex items-center justify-between ">
            <div @click="isOpen=!isOpen" class="z-50 md:hidden  absolute h-20 w-full " />
            <transition name="fade-blur">
                    <img :src="music[musicPosition].cover" alt="" class="absolute opacity-30 pr-20 blur-2xl z-0 h-7 w-[222rem]">
                </transition>
               

                    <div class="flex h-20 w-full  md:items-center justify-start   gap-4 z-0 text-white">
                    <img :src="music[musicPosition].cover" alt="" class="md:h-14 md:w-14  rounded- md:rounded-none flex">
                    <div class="flex flex-col justify-center items-start ">
                        <p>{{music[musicPosition].title}}</p>
                        <p class="text-neutral-500">{{music[musicPosition].artist}}</p>
                    </div>
                </div>
                <div class="hidden md:flex  h-20 w-[30vw]  items-center justify-center gap-4 z-30 text-white  transition-all  opacity-0 md:group-hover:opacity-100 ">
                    <img src="../assets/icon/previous.svg" alt="" class="h-5 w-5 cursor-pointer" @click="previous()">
                    <img v-if="isPlaying" src="../assets/icon/stop.svg" alt="" class="h-5 w-5 cursor-pointer" @click="stop()">
                    <img v-else src="../assets/icon/play.svg" alt="" class="h-5 w-5 cursor-pointer" @click="play()">
                    <img src="../assets/icon/next.svg" alt="" class="h-5 w-5 cursor-pointer" @click="next()">
                </div>   
        
            <div class=" flex h-20 w-fit md:w-full items-center justify-end gap-2  text-white  z-50 ">
                <div  class="  md:group-hover:hidden text-sm font-bold text-neutral-500"">
                    <p class="hidden md:flex">02:30</p>
                    <div class=" flex gap-4 justify-center items-center md:hidden  z-50">
                        <img src="../assets/icon/previous.svg" alt="" class="h-5 w-5 cursor-pointer" @click="previous()">
                        <img v-if="isPlaying" src="../assets/icon/stop.svg" alt="" class="h-5 w-5 cursor-pointer" @click="stop()">
                        <img v-else src="../assets/icon/play.svg" alt="" class="h-5 w-5 cursor-pointer" @click="play()">
                        <img src="../assets/icon/next.svg" alt="" class="h-5 w-5 cursor-pointer" @click="next()">
                    </div>
                </div>
                <div class=" md:group-hover:flex hidden gap-6 items-center justify-center">
                    <div class="h-[4px] w-20 bg-neutral-500 rounded-full overflow-hidden ">
                        <div class="h-full w-1/2 bg-white "></div>
                    </div>
                    <img src="../assets/icon/volumeUp.svg" alt="" class="h-5 w-5 ">
                </div>
            </div>
        </div>

   
    
    

    </div>
    <transition >
    <div v-if="isOpen" class="  flex-col h-screen rounded-3xl w-screen bg-neutral-900 fixed flex justify-end p-6 ">
        <div class="w-full flex items-start h-20 justify-end">
            <img @click="isOpen=!isOpen" src="../assets/icon/down.svg" class="text-white h-6 z-20"/>

        </div>
        <div class="h-full w-full flex text-white flex-col gap-6">
            <div>
                <img :src="music[musicPosition].cover" alt="" class="absolute rounded-xl opacity-20 scale-150 blur-xl z-0">
                <img :src="music[musicPosition].cover" alt="" class="  z-10  rounded-3xl flex">

            </div>
            <div class="flex flex-col justify-center items-start ">
                <p>{{music[musicPosition].title}}</p>
                <p class="text-neutral-500">{{music[musicPosition].artist}}</p>
            </div>
            <div class=" flex gap-4 justify-center items-center md:hidden  z-50">
                <img src="../assets/icon/previous.svg" alt="" class="h-8 w-8 cursor-pointer" @click="previous()">
                <img v-if="isPlaying" src="../assets/icon/stop.svg" alt="" class="h-8 w-8 cursor-pointer" @click="stop()">
                <img v-else src="../assets/icon/play.svg" alt="" class="h-8 w-8 cursor-pointer" @click="play()">
                <img src="../assets/icon/next.svg" alt="" class="h-8 w-8 cursor-pointer" @click="next()">
            </div>
            <div class="w-[20rem] h-1 text-xs flex justify-between">
                <p>02:00</p>
                <p>04:00</p>
            </div>
            <div class="flex flex-col relative">
                
                <div class="z-0 absolute w-[20rem] h-[1px] bg-neutral-600 rounded-full">
                </div>
                <div class="z-20 absolute w-[10rem] h-[1px] bg-white rounded-full">
                </div>


            </div>

        </div>

    </div>
    </transition>
</template>

<style>
    .fade-blur-enter-active, .fade-blur-leave-active {
        transition: opacity 0.5s, filter 0.5s;
    }
    .fade-blur-enter, .fade-blur-leave-to {
        opacity: 0;
        filter: blur(10px);
    }
    .v-enter-active,
    .v-leave-active {
      transition: all 0.2s ease;
    }
    
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
      transform: translateY(200%);
    }


    </style>
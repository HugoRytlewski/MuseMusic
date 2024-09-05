<script setup>
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
const slide = ref(0);


      let music = [
      {
        title: "La vie qu'on mène",
        artist: "Ninho",
        duration: "03:04",
        cover: "/img/ni.jpg",
        src: "/music/b.mp3",
      },
      {
        title: "Eurostar",
        artist: "Ninho",
        duration: "03:04",
        cover: "/img/ni2.jpg",
        src: "/music/a.mp3",
      },
      
    ];
</script>
<template>
 
    <div class="h-screen w-screen p-10 items-center justify-center flex cursor-grab active:cursor-grabbing">
        <div class="">
        
            <div class="absolute h-screen overflow-hidden w-screen flex items-center justify-center z-0">
                <img :src="music[slide].cover" class=" blur-xl z-20 scale-150  opacity-20 object-cover h-screen w-screen  object-bottom">

            </div>
            <div class="h-28 w-screen rounded-xl pl-20 pr-20 pt-10 z-40 top-10 absolute cursor-pointer flex items-center justify-center">
              <div class="h-full w-[40rem] bg-neutral-400 bg-opacity-25 rounded-full items-center justify-start flex">
                <img src="../assets/icon/search.svg" alt="" class="h-6 absolute translate-x-7 ">
                <input type="text" class="focus:outline-none focus:outline-2 text-2xl pl-20 focus:outline-neutral-200 focus:outline-opacity-30 h-full w-full p-4 bg-transparent appearance-none rounded-full" placeholder="Musique, Artiste, Albums" >
              </div>

            </div>

                <div 
                >
                <swiper 
                class="  h-screen w-screen flex items-center justify-center  "
                :space-between=-1100
                :slides-per-view="1"
                @slideChange="slide = $event.activeIndex"


                >
                    <swiper-slide  class="flex items-center justify-center  "  
                    v-for="m in music"
                    > 
                    <div class="h-screen w-screen flex flex-col items-center justify-center">

                        <img :src="m.cover" alt="" class="absolute z-20 w-96 h-96  ">

                        <div class="translate-y-60 flex flex-col items-center ">
                            <p class="text-xl z-50 font-bold"> {{m.title}}</p>
                            <p class="text-xl z-50 font-bold text-neutral-400"> {{m.artist}}</p>

                        </div>

                    </div>

                    </swiper-slide>


              </swiper>
            </div>

    </div>
</div>

</template>
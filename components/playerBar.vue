<script setup>
    import { ref, onMounted, watch } from 'vue';
    
    const isPlaying = ref(false);
    const volume = ref(50);
    const musicPosition = ref(0);
    const isOpen = ref(false);
    const currentTime = ref(0);
    const duration = ref(0);   
    const isLiked = ref(false);
    const isRandom = ref(false);
    const isLoop = ref(false);
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
      {
        title: "Zinzin",
        artist: "H.E.M",
        duration: "01:32",
        cover: "/img/zinzin.jpg",
        src: "/music/c.mp3",
        
      }
    ];
    
    let audio;
    
    onMounted(() => {
      audio = new Audio();
      audio.addEventListener('loadedmetadata', () => {
    duration.value = audio.duration;
  });

  audio.addEventListener('timeupdate', () => {
    currentTime.value = audio.currentTime;
  });
});

    
    function play() {
      if (!audio.src) {
        audio.src = music[musicPosition.value].src;
      }
    
      if (isPlaying.value) {
        audio.pause();
        isPlaying.value = false;
      } else {
        audio.volume = volume.value / 100;
        audio.play();
        isPlaying.value = true;
      }
    }
    
    function setVolume(numb) {
      volume.value = Math.min(Math.max(numb, 0), 100);
      if (audio) audio.volume = volume.value / 100;
    }
    
    function changeTrack(newPosition) {
      if (isPlaying.value) {
        audio.pause();
      }
      musicPosition.value = newPosition;
      audio.src = music[musicPosition.value].src;
      audio.volume = volume.value / 100;
      audio.play();
      isPlaying.value = true;
    }
    
    function next() {
      let newPosition = (musicPosition.value + 1) % music.length;
      changeTrack(newPosition);
    }
    
    function previous() {
      let newPosition = (musicPosition.value - 1 + music.length) % music.length;
      changeTrack(newPosition);
    }
    
    watch(volume, (newValue) => {
      if (audio) audio.volume = newValue / 100;
    });

    function formatTime(seconds) {
  const minutes = Math.floor(seconds / 60);
  const secs = Math.floor(seconds % 60);
  return `${String(minutes).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
}

function changeTime(time) {
    audio.currentTime = time;
    }

    onMounted(() => {
    for (let e of document.querySelectorAll('input[type="range"].slider-progress')) {
        e.style.setProperty('--value', e.value);
        e.style.setProperty('--min', e.min == '' ? '0' : e.min);
        e.style.setProperty('--max', e.max == '' ? '100' : e.max);
        e.addEventListener('input', () => e.style.setProperty('--value', e.value));
      }
});

function random() {
  
  if (isRandom.value) {
    isRandom.value = false;
    let currentMusic = music[musicPosition.value];
    music = music.filter((m, i) => i !== musicPosition.value);
    music = [currentMusic, ...music];
      } else {

    isRandom.value = true;
    let currentMusic = music[musicPosition.value];
    music = music.filter((m, i) => i !== musicPosition.value);
    music = [currentMusic, ...music.sort(() => Math.random() - 0.5)];
    musicPosition.value = 0;
    


    
  }
}

function loop(){
  if (isLoop.value) {
    isLoop.value = false;
    audio.loop = false;
  } else {
    isLoop.value = true;
    audio.loop = true;
  }
}


  
   
</script>


<template>
    <div class="fixed bottom-0 h-20 left-1/2 flex items-center justify-center group">
        <div  class="fixed  bottom-0  h-[3.5rem] md:h-20 md:w-[20rem] md:group-hover:w-[45rem] overflow-hidden w-[90vw]  duration-300 mb-3 md:mb-6  rounded-full md:pl-10 pr-10 bg-neutral-900 flex items-center justify-between ">
            <div @click="isOpen=!isOpen" class="z-50 md:hidden  absolute h-20 w-full " />
            <transition name="fade-blur">
                    <img :src="music[musicPosition].cover" alt="" class="absolute opacity-30 pr-20 blur-xl scale-150  z-0 h-7 w-[222rem]">
                </transition>

                    <div class="flex h-20 w-full  md:items-center justify-start   gap-4 z-0 text-white">
                      <div class="bg-neutral-500 absolute h-14 w-14 bg-opacity-0 group/i hover:bg-opacity-25 transition-all flex items-center justify-center"> 
                        <img @click="isLiked=!isLiked" v-if="isLiked" src="../assets/icon/like.svg" alt="" class="h-6 group-hover/i:opacity-100 opacity-0 cursor-pointer transition-all">
                        <img @click="isLiked=!isLiked" v-if="!isLiked" src="../assets/icon/liked.svg" alt="" class="h-6 group-hover/i:opacity-100 opacity-0 cursor-pointer transition-all">

                      </div>
                    <img :src="music[musicPosition].cover" alt="" class="md:h-14 md:w-14  rounded- md:rounded-none flex">
                    <div class="flex flex-col justify-center items-start ">
                        <p class="w-32 line-clamp-1 group-hover:w-[9rem] overflow-hidden h-6 ">{{music[musicPosition].title}}</p>
                        <p class="text-neutral-500">{{music[musicPosition].artist}}</p>
                    </div>
                </div>
                <div class="hidden md:flex flex-col h-20 w-[30vw]  items-center justify-center z-30 text-white  transition-all  opacity-0 md:group-hover:opacity-100 ">
                   <div class="flex gap-4 items-center ">
                    <img 
                    @click="random()" 
                    src="../assets/icon/random.svg" 
                    class="h-6 w-6 rounded-full p-1 cursor-pointer"                 
                    :class="{'bg-neutral-600': isRandom}"
                    alt=""
                    >
                    <img src="../assets/icon/previous.svg" alt="" class="h-5 w-5 cursor-pointer" @click="previous()">
                    <img v-if="isPlaying" src="../assets/icon/stop.svg" alt="" class="h-5 w-5 cursor-pointer" @click="play(false)">
                    <img v-else src="../assets/icon/play.svg" alt="" class="h-5 w-5 cursor-pointer" @click="play(true)">
                    <img src="../assets/icon/next.svg" alt="" class="h-5 w-5 cursor-pointer" @click="next()">
                    <img 
                    @click="loop()" 
                    src="../assets/icon/loop.svg" 
                    class="h-6 w-6 rounded-full p-1 cursor-pointer"                 
                    :class="{'bg-neutral-600': isLoop}"
                    alt=""
                    >
                </div>
                <div class="relative h-5 w-full flex items-end select-none rounded-xl">
                    <input type="range" min="0" :max="duration"  v-model="currentTime" @input="changeTime(currentTime)" class=" w-full absolute h-1 rounded-xl bg-neutral-800 z-30"  id="myRange" >
                    <div class=" absolute h-1 rounded-xl bg-white z-30" :style="{ width: `${currentTime / duration * 100}%` }"></div>
                    <div class=" absolute h-1 rounded-xl bg-neutral-600 z-0 w-full" ></div>
    
                </div>

                </div>   

            <div class=" flex h-20 w-fit md:w-full items-center justify-end gap-2  text-white  z-50 ">
                <div  class="  md:group-hover:hidden text-sm font-bold text-neutral-500"">
                    <p class="hidden md:flex">{{ formatTime(currentTime) }}</p>
                    <div class=" flex gap-4 justify-center items-center md:hidden  z-50">
                        <img src="../assets/icon/previous.svg" alt="" class="h-5 w-5 cursor-pointer" @click="previous()">
                        <img v-if="isPlaying" src="../assets/icon/stop.svg" alt="" class="h-5 w-5 cursor-pointer" @click="play(false)">
                        <img v-else src="../assets/icon/play.svg" alt="" class="h-5 w-5 cursor-pointer" @click="play(true)">
                        <img src="../assets/icon/next.svg" alt="" class="h-5 w-5 cursor-pointer" @click="next()">

                    </div>

                </div>
                <div class=" md:group-hover:flex hidden gap-6 items-center justify-center">
                    
                    <div class="relative h-5 w-20 flex items-center select-none ">
                        <input type="range" min="0" :max="100"  v-model="volume"  class=" cursor-pointer w-full absolute h-1 rounded-xl bg-neutral-800 z-30"  id="myRange" >
                        <div class=" absolute h-1 rounded-xl bg-white z-30" :style="{ width: `${volume / 100 * 100}%` }"></div>
                        <div class=" absolute h-1 rounded-xl bg-neutral-500 z-0 w-full" ></div>
        
                    </div>
                    <img v-if="volume >= 50" src="../assets/icon/volumeUp.svg" alt="" class="h-5 w-5 ">
                    <img v-if="volume<50 && volume > 1" src="../assets/icon/volumedown.svg" alt="" class="h-5 w-5 ">
                    <img v-if="volume==0" src="../assets/icon/volumeOff.svg" alt="" class="h-5 w-5 ">

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
            <div class=" flex  justify-center ">
                <img :src="music[musicPosition].cover" alt="" class="absolute rounded-xl opacity-20 scale-150 blur-xl z-0">
                <img :src="music[musicPosition].cover" alt="" class="  z-10 h-[18rem] w-[18rem] object-cover  rounded-3xl flex">

            </div>
            <div>
                <div class="flex flex-col justify-center items-start font-bold text-2xl ">
                    <p>{{music[musicPosition].title}}</p>
                    <p class="text-neutral-500">{{music[musicPosition].artist}}</p>
                </div>
            </div>
           
            <div class=" flex gap-4 justify-center items-center md:hidden  z-50">
                <img src="../assets/icon/next.svg" alt="" class="h-8 w-8 cursor-pointer rotate-180" @click="previous()">
                <img v-if="isPlaying" src="../assets/icon/stop.svg" alt="" class="h-8 w-8 cursor-pointer" @click="play(false)">
                <img v-else src="../assets/icon/play.svg" alt="" class="h-8 w-8 cursor-pointer" @click="play(true)">
                <img src="../assets/icon/next.svg" alt="" class="h-8 w-8 cursor-pointer" @click="next()">
            </div>
            <div class="w-full h-1 text-xs flex justify-between">
                <p>{{ formatTime(currentTime) }}</p>
                <p>{{ formatTime(duration) }}</p>
            </div>
        

            <div class="relative flex items-end select-none">
                <input type="range" min="0" :max="duration"  v-model="currentTime" @input="changeTime(currentTime)" class=" w-full absolute h-2 rounded-xl bg-neutral-800 z-30"  id="myRange" >
                <div class=" absolute h-2 rounded-xl bg-white z-30" :style="{ width: `${currentTime / duration * 100}%` }"></div>
                <div class=" absolute h-2 rounded-xl bg-neutral-800 z-0 w-full" ></div>
            </div>

            <div class="flex items-center justify-between gap-6 select-none">
                <div 
                @click="random()" 
                class="h-10 w-fit  active:bg-neutral-400 flex items-center justify-center p-2 rounded-full"
                :class="{'bg-neutral-500': isRandom , 'bg-neutral-700': !isRandom}"
                
                >
                
                <img src="../assets/icon/random.svg" alt="" class="h-6">
                </div>
                <div v-if="!isLiked" @click="isLiked=!isLiked" class="h-10 w-fit active:bg-neutral-400 bg-neutral-700 flex items-center justify-center p-2 rounded-full">
                    <img src="../assets/icon/like.svg" alt="" class="h-6">
                </div>
                <div v-else @click="isLiked=!isLiked" class="h-10 w-fit active:bg-neutral-400 bg-neutral-700 flex items-center justify-center p-2 rounded-full">
                    <img src="../assets/icon/liked.svg" alt="" class="h-6">
                </div>
                    <img 
                    @click="loop()" 
                    src="../assets/icon/loop.svg" 
                    alt="" 
                    class="h-10 w-fit  active:bg-neutral-400 flex items-center justify-center p-2 rounded-full"
                    :class="{'bg-neutral-500': isLoop , 'bg-neutral-700': !isLoop}"

                    >
                
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
      transition: all 0.5s ease;
    }
    
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
      transform: translateY(200%);
    }


    </style>
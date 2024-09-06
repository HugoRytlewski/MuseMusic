<script setup>
  import { ref, computed, watch } from 'vue';
  import { Swiper, SwiperSlide } from 'swiper/vue';
  import { Keyboard } from 'swiper/modules';
  import { useWindowSize } from '@vueuse/core'
  import 'swiper/css';
  import 'swiper/css/keyboard';

  const { width, height } = useWindowSize();
  const spaceBetween = width.value > 500 ? -1300 : (width.value > 400 ? -160 : -150);
  const modules = [Keyboard];
  const slide = ref(0);
  let music = ref([
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
  ]);
  
  const searchQuery = ref('');
  
  const filteredMusic = computed(() => {
    const query = searchQuery.value.toLowerCase();
  
    if (!query) {
      return music.value; 
    } else {
      return music.value.filter(m =>
        m.title.toLowerCase().includes(query) ||
        m.artist.toLowerCase().includes(query)
      );
    }
  });
  
  watch(filteredMusic, (newValue) => {
    if (newValue.length === 0) {
      slide.value = 0;
    }
  });


  </script>
  
  <template>
    <div class="h-screen w-screen p-10 items-center justify-center flex cursor-grab active:cursor-grabbing">
      <div>

        <!-- Image de fond avec message -->
        <div class="absolute h-screen overflow-hidden w-screen flex items-center justify-center z-0">
          
          <!-- Message si aucune musique ne correspond à la recherche -->
          <div v-if="filteredMusic.length === 0" class="absolute top-0 left-0 flex text-center text-white mt-10 pb-[30rem] h-screen w-screen  items-center justify-center">
            <p>Aucune musique trouvée pour "{{ searchQuery }}".</p>
          </div>
          
          <!-- Image de fond -->
          <img 
          :src="filteredMusic.length ? filteredMusic[slide].cover : '/img/default-cover.jpg'" 
          class="blur-xl z-20 scale-150 opacity-20 object-cover h-screen w-screen object-bottom"
          >  
        </div>
  
        <!-- Barre de recherche -->
        <div class="h-28 w-screen rounded-xl pl-20 pr-20 pt-10 z-40 top-10 absolute cursor-pointer flex items-center justify-center">
          <div class="md:h-full h-14 w-[40rem] bg-neutral-400 bg-opacity-25 rounded-full items-center justify-start flex">
            <img src="../assets/icon/search.svg" alt="" class="h-6 absolute translate-x-7">
            <input 
              type="text" 
              v-model="searchQuery" 
              class="focus:outline-none focus:outline-2 md:text-2xl pl-20 focus:outline-neutral-200 focus:outline-opacity-30 h-full w-full p-4 bg-transparent appearance-none rounded-full" 
              placeholder="Musique, Artiste, Albums">
          </div>
        </div>
  
        <!-- Swiper -->
        <div>
          <swiper 
            :modules="modules"
            :keyboard="{enabled: true}"
            class="h-screen w-screen flex items-center justify-center mySwiper"
            :space-between="spaceBetween"
            :slides-per-view="1"
            @slideChange="slide = $event.activeIndex"
          >
            <!-- Affichage des musiques  -->
            <swiper-slide 
              class="flex items-center justify-center "  
              v-for="(m,i) in filteredMusic" 
              :key="m.title"
            > 
              <div class=" h-screen w-screen flex flex-col items-center justify-center gap-10 z-20">
                <div class="group h-96 w-96 flex flex-col items-center justify-center z-20 ">
                  <img 
                  :src="m.cover" 
                  alt="" 
                  class=" group-hover:opacity-40  z-20 md:w-96 md:h-96 h-52 w-52 transition-all duration-500 "
                  :class="{ 'opacity-50': slide != i, 'opacity-100 scale-110': slide == i }"
                  >
                  <img v-if="slide == i" src="../assets/icon/play.svg" alt="" class=" cursor-pointer opacity-0 group-hover:opacity-100  duration-500 absolute h-20 z-30">
                 
                  
                </div>
                <transition name="fade">
                  <div  v-if="slide == i" class="absolute md:translate-y-64 translate-y-44 flex flex-col items-center transition-all duration-300 ">
                    <p class="text-xl z-50 font-bold">{{ m.title }}</p>
                    <p  class="text-xl z-50 font-bold text-neutral-400">{{ m.artist }}</p>
                  </div>
                </transition>
              
              </div>
            </swiper-slide>
          </swiper>
          
        
          
        </div>
      </div>
    </div>
  </template>

  <style>
  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.5s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }

  </style>
  
<script setup>
  import { ref, computed, watch } from 'vue';
  import { Swiper, SwiperSlide } from 'swiper/vue';
  import 'swiper/css';
  
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
  
  // Variable pour la recherche
  const searchQuery = ref('');
  
  // Filtrage des musiques selon la recherche
  const filteredMusic = computed(() => {
    const query = searchQuery.value.toLowerCase();
  
    if (!query) {
      return music.value; // Retourne la liste entière si aucune recherche n'est effectuée
    } else {
      return music.value.filter(m =>
        m.title.toLowerCase().includes(query) ||
        m.artist.toLowerCase().includes(query)
      );
    }
  });
  
  // Réinitialiser Swiper si filteredMusic change
  watch(filteredMusic, (newValue) => {
    if (newValue.length === 0) {
      slide.value = 0;  // Réinitialiser l'index si la recherche ne donne rien
    }
  });
  </script>
  
  <template>
    <div class="h-screen w-screen p-10 items-center justify-center flex cursor-grab active:cursor-grabbing">
      <div>
        <!-- Image de fond -->
        <div class="absolute h-screen overflow-hidden w-screen flex items-center justify-center z-0">
          <div v-if="filteredMusic.length === 0" class="absolute top-0 left-0 flex text-center text-white mt-10 pb-[30rem] h-screen w-screen  items-center justify-center">
            <p>Aucune musique trouvée pour "{{ searchQuery }}".</p>
          </div>
                    <img :src="filteredMusic.length ? filteredMusic[slide].cover : '/img/default-cover.jpg'" 
               class="blur-xl z-20 scale-150 opacity-20 object-cover h-screen w-screen object-bottom">
               
        </div>
  
        <!-- Barre de recherche -->
        <div class="h-28 w-screen rounded-xl pl-20 pr-20 pt-10 z-40 top-10 absolute cursor-pointer flex items-center justify-center">
          <div class="h-full w-[40rem] bg-neutral-400 bg-opacity-25 rounded-full items-center justify-start flex">
            <img src="../assets/icon/search.svg" alt="" class="h-6 absolute translate-x-7">
            <input 
              type="text" 
              v-model="searchQuery" 
              class="focus:outline-none focus:outline-2 text-2xl pl-20 focus:outline-neutral-200 focus:outline-opacity-30 h-full w-full p-4 bg-transparent appearance-none rounded-full" 
              placeholder="Musique, Artiste, Albums">
          </div>
        </div>
  
        <!-- Swiper -->
        <div>
          <swiper 
            class="h-screen w-screen flex items-center justify-center"
            :space-between="-1100"
            :slides-per-view="1"
            @slideChange="slide = $event.activeIndex"
          >
            <!-- Affichage des musiques filtrées -->
            <swiper-slide 
              class="flex items-center justify-center"  
              v-for="m in filteredMusic" 
              :key="m.title"
            > 
              <div class="h-screen w-screen flex flex-col items-center justify-center">
                <img :src="m.cover" alt="" class="absolute z-20 w-96 h-96">
                <div class="translate-y-60 flex flex-col items-center">
                  <p class="text-xl z-50 font-bold">{{ m.title }}</p>
                  <p class="text-xl z-50 font-bold text-neutral-400">{{ m.artist }}</p>
                </div>
              </div>
            </swiper-slide>
          </swiper>
          
          <!-- Message si aucune musique ne correspond à la recherche -->
        
          
        </div>
      </div>
    </div>
  </template>
  
<template lang="">
    <div class="w-full flex flex-col items-start gap-5 bg-gray-800 h-full p-4 max-sm:p-2 overflow-y-scroll">
        <!-- Playlist Title -->
        
        <div class="flex items-center justify-center w-full my-6">
            <span class="text-3xl font-sans">{{playList?.[0]?.track?.name}}</span>
        </div>

        <div class="w-[80%] max-sm:w-full mx-auto flex max-sm:flex-col gap-2 max-sm:gap-5">
            <!-- List -->
        <div class="w-[50%] max-sm:w-full h-[fit-content] flex flex-col gap-3 items-center border-r-2 border-gray-600">
            <!-- List Header -->
            <div class="w-full border-b-2 border-b-gray-400 py-2 flex justify-between items-center px-4 max-sm:px-1">
                <div class="text-gray-400 flex gap-2 max-sm:text-sm">
                    <span>#</span>
                    <span>Title</span>
                </div>

                <span class="text-gray-400 max-sm:text-sm">Plays</span>

                <div class="text-gray-400">
                    <IconDuration cls="w-[25px] h-[25px]" />
                </div>
            </div>
            <!-- List Header -->

            <!-- List Body -->
           
            <div v-if="playList?.[0]?.track?.playcount/1000000 !== 'NaN'" @click="selectSong()" class="w-full py-2 flex justify-between items-center px-4 max-sm:px-1 cursor-pointer hover:bg-gray-950 rounded-md">
               <div class="flex gap-2 text-gray-400">
                    <span class="max-sm:text-sm">1</span>

                    <div class="w-full flex max-sm:flex-col gap-3 max-sm:gap-1 justify-start items-center max-sm:items-start rounded-md  max-sm:p-1 cursor-pointer">
                    <div class="w-[50px] h-[50px] max-sm:w-[35px] max-sm:h-[35px] rounded-md overflow-hidden">
                        <img class="w-full h-full object-contain" src="https://media.istockphoto.com/id/457945109/photo/hands-of-a-dj-mixing-music-at-disco.jpg?s=612x612&w=0&k=20&c=zFshaN_FtucyUvNCCrBmxmHhECvIrvJ3PucAHdK49no=" />
                    </div>
                    <div class="flex flex-col gap-3 max-sm:gap-1.5">
                        <span class="text-white max-sm:text-sm">{{playList?.[0]?.track?.name}}</span>
                        <div class="flex items-center gap-1">
                            <span v-for="artist in playList?.[0]?.track?.artists?.items" class="text-gray-400 text-sm">{{artist?.profile.name}}</span>
                        </div>
                    </div>
                </div>
               </div>

                <span class="text-gray-400 max-sm:text-sm">{{`${playList?.[0]?.track?.playcount/1000000}M`}}</span>

                <span class="text-gray-400 max-sm:text-sm">{{fromatMilliSec(playList?.[0]?.track?.duration?.totalMilliseconds)}}</span>
            </div>
            <!-- List Body -->
        </div>
        <!-- List -->

        <div class="w-[50%] max-sm:w-full rounded-lg bg-gray-950 border-[1px] border-gray-600 p-2">
            <LayoutPlayer v-if="currentSong" :currentSong="currentSong" />
        </div>
        </div>

        <!-- Section Footer -->
        <span class="text-gray-400 text-base lg:text-lg lg:font-semibold font-normal mt-1 mb-10 lg:mb-14 pb-2 text-center border-r-2 border-gray-600 w-full">&copy; 2023 Muse</span>
        <!-- Section Footer -->
    </div>
</template>
<script setup>
const preview_url = ref('');
const playList = ref('');
const playListImg = ref('');
const state = ref('pending');
const currentSong = ref(null);

const { id } = useRoute().params;
console.log("id", id)
const trackId = id[0];
console.log(trackId);

const fullPath = useRoute().fullPath;
const urlRegex = /https?:\/\/\S+/;

preview_url.value = fullPath.match(urlRegex)[0];


const fromatMilliSec = (milliseconds) => {
    const seconds = Math.floor(milliseconds / 1000);
    const minutes = Math.floor(seconds / 60);
    const remainingSeconds = seconds % 60;
    const formattedDuration = `${minutes}:${remainingSeconds < 10 ? '0' : ''}${remainingSeconds} s`;
    return formattedDuration;
}

const getPlayList = async () => {
    console.log('getting playlist')
   const url = `https://spotify81.p.rapidapi.com/album_tracks?id=${trackId}&offset=0&limit=300`;
    const options = {
        method: 'GET',
        headers: {
            'X-RapidAPI-Key': '7cfe79fb4cmsha97c9c0ea2a5ac4p1fd173jsn0f3c71c9c278',
            'X-RapidAPI-Host': 'spotify81.p.rapidapi.com'
        }
    };

    try {
        state.value = 'loading';
        const response = await fetch(url, options);
        const result = await response.json();
        playList.value = result?.data?.album?.tracks?.items;
        console.log(playList.value)

    } catch (error) {
        console.error(error);
    } finally {
        state.value = 'not-loading';
    }
}

const getPlayListImg = async () => {
   const url = `https://spotify81.p.rapidapi.com/tracks?ids=${trackId}`;
    const options = {
        method: 'GET',
        headers: {
            'X-RapidAPI-Key': '7cfe79fb4cmsha97c9c0ea2a5ac4p1fd173jsn0f3c71c9c278',
            'X-RapidAPI-Host': 'spotify81.p.rapidapi.com'
        }
    };

    try {
        state.value = 'loading';
        const response = await fetch(url, options);
        const result = await response.json();
        // const matchItem = result?.items?.find(item => item?.track?.album?.id == trackId);
        // console.log(result?.items?.[0]?.track?.album?.id)
        // playListImg.value = matchItem?.album?.images?.[0]?.url;
        playListImg.value = result?.tracks?.[0]?.album?.images?.[0]?.url;
        console.log(playListImg.value)
    } catch (err) {
        console.log(err)
    }
}

onBeforeMount(() => {
    if (!localStorage.getItem('loggedinUser')) {
        router.push('/login')
    }

    
     getPlayList();
    getPlayListImg();
    if (playList?.value?.[0]?.track?.playcount / 1000000 === 'NaN') {
        console.log('NAn')
        getPlayList();
        getPlayListImg();
    }
    console.log(currentSong.value)

     currentSong.value = {
        preview_url: preview_url.value,
        name: playList.value?.[0]?.track?.name,
        duration: fromatMilliSec(playList.value?.[0]?.track?.duration?.totalMilliseconds)
    }
})

onMounted(() => {
    getPlayList();
    getPlayListImg();
    if (playList?.value?.[0]?.track?.playcount / 1000000 === 'NaN') {
        getPlayList();
        getPlayListImg();
    }
   
    
})




</script>
<style lang="">

    
</style>
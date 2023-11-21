<template lang="">
    <div class="w-full h-full flex items-center justify-between">
        <div v-if="!currentSong?.name" class="fonts-sans font-semibold text-lg">song</div>
        <div class="fonts-sans font-semibold text-lg">{{currentSong?.name}}</div>

       <div>
                <video ref="videoPlayer" class="w-[1px] h-[1px]" controls="" autoplay="" name="media"><source :src="currentSong?.preview_url" type="audio/mpeg"></video>
                <IconPlay v-if="!isPlaying" @click="toggleVideo" color="text-white" cls="w-[50px] h-[50px] text-white cursor-pointer active:w-[45px] active:h-[45px]" />
                <IconPause v-else @click="toggleVideo" color="text-white" cls="w-[50px] h-[50px] text-white cursor-pointer active:w-[45px] active:h-[45px]" />
            </div>

            <!-- Volume Controls -->
            <div v-if="isPlaying" class="w-[45px] h-[45px] max-sm:w-[35px] max-sm:h-[35px] bg-transparent">
                <IconPlayIndicator cls="w-full h-full" color="white" />
            </div>
            <div v-else class="w-[45px] h-[45px] max-sm:w-[35px] max-sm:h-[35px] bg-transparent">
                <IconPauseIndicator cls="w-full h-full" color="white" />
            </div>
    </div>
</template>
<script setup>
const props = defineProps(['currentSong']);

const videoPlayer = ref(null);

const isPlaying = ref(true);

const toggleVideo = () => {
    console.dir(videoPlayer.value);
    if (isPlaying.value) {
        videoPlayer.value.pause();
    } else {
        videoPlayer.value.play();
    }

    isPlaying.value = !isPlaying.value;
};


</script>
<style lang="">
    
</style>

<template>
    <div :class="`fixed top-0 ${opened ? 'left-0' : 'left-[-100%]'} z-10 flex flex-col lg:hidden gap-3 bg-black w-[75%] h-[100vh] text-white pt-5 px-2`">
    <div class="flex gap-5 flex-col items-start">
            <nuxt-link to="/recents" class="font-bold text-lg max-sm:text-md">Recents</nuxt-link>

                <nuxt-link to="/search"
                  class="block w-full rounded-md border-0 py-1.5 text-gray-400 bg-transparent shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 p-2">Search</nuxt-link>
            </div>

            <div class="flex gap-1 items-center font-bold max-sm:text-md">
                <nuxt-link to="/login">Login</nuxt-link>|
                <nuxt-link to="/signup">Signup</nuxt-link>
                <!-- <nuxt-link v-if="" to=""></nuxt-link> -->
            </div>

            </div>
    <div v-if="opened" @click="onPress" class="max-sm:fixed top-0 right-0 w-[100%] h-[100vh] z-[9] bg-transparent lg:hidden cursor-pointer"></div>
</template>
<script setup>

const props = defineProps(['opened', 'onPress'])
const recents = ref(null);

const state = ref('pending');
</script>
<style lang="">
</style>
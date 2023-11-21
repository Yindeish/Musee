<template lang="">
    <div class="w-full h-[fit-content] bg-black border-b-[1px] border-gray-600 flex justify-between items-center px-3 py-2 font-sans">

      <nuxt-link to="/" class="font-black text-3xl max-sm:text-lg">Muse</nuxt-link>

      <div class="max-sm:hidden mt-2 flex gap-5 items-center">
        <nuxt-link to="/recents" class="font-bold text-lg max-sm:text-md">Recents</nuxt-link>

            <nuxt-link to="/search"
              class="block rounded-md border-0 py-1.5 text-gray-400 bg-transparent shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 p-2 w-[8rem]">Search</nuxt-link>
        </div>

        <div v-if="!loggedin" class="max-sm:hidden flex gap-1 items-center font-bold max-sm:text-md">
            <nuxt-link  to="/login">Login</nuxt-link>|
            <nuxt-link to="/signup">Signup</nuxt-link>
        </div>

        <span class="max-sm:hidden flex gap-1 items-center font-bold max-sm:text-md" v-if="loggedin" @click="signout">Signout</span>

        <span @click="open" class="lg:hidden text-white cursor-pointer active:scale-95">
            <IconHamburger cls="text-white w-[25px] h-[25px]" />
        </span>

        <AppSideBar :opened="opened" :onPress="close" />
    </div>
</template>
<script setup>

const opened = ref(true);
const props = defineProps(['onPress']);
const router = useRouter();
const loggedin = ref(false);

onBeforeMount(() => {
    if (!localStorage.getItem('loggedinUser')) {
        router.push('/login')
    } else {
        loggedin.value = true;
    }
})

const signout = () => {
    localStorage.removeItem('loggedinUser');
}

const currentSegment = ref('');

const open = () => {
    opened.value = true;
}

const close = () => {
    opened.value = false;
}

const forward = () => {
    router.forward()
}
const backward = () => {
    router.back()
}

onMounted(() => {
    const segment = useRequestURL().pathname;
    currentSegment.value = segment.split("/")[1];
})

</script>
<style lang="">
    
</style>
<template lang="">
    <div class="w-full h-[fit-content] bg-black border-b-[1px] border-gray-600 flex justify-between items-center px-3 py-2 font-sans">

      <span class="font-black text-3xl">Muse</span>

      <div class="mt-2 flex gap-5 items-center">
        <nuxt-link to="/re" class="font-bold text-lg">Recents</nuxt-link>

            <input id="text" name="text" type="text" placeholder="Search playlist"
              class="block w-full rounded-md border-0 py-1.5 text-gray-400 bg-transparent shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 p-2">
        </div>

        <div class="flex gap-1 items-center font-bold">
            <nuxt-link to="/login">Login</nuxt-link>
            <nuxt-link to="/signup">Signup</nuxt-link>
            <!-- <nuxt-link v-if="" to=""></nuxt-link> -->
        </div>
    </div>
</template>
<script setup>

const props = defineProps(['onPress']);
const router = useRouter();

const currentSegment = ref('');

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
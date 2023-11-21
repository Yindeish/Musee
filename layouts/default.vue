<template>
    <div class="bg-black w-full h-[100vh] m-0 font-sans text-white flex flex-col gap-3">
        <AppHeader />
        <slot></slot>
    </div>
</template>

<script setup>
const router = useRouter();

const opened = ref(false);

const openSideBar = () => {
    opened.value = true;
}

const closeSideBar = () => {
    opened.value = false;
}

onBeforeMount(() => {
    console.log('before munt');
    if (!localStorage.getItem('loggedinUser')) {
        router.push('/login');
    }

})

</script>

<style></style>

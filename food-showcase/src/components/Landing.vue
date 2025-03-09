<template>
    <div class="relative w-full h-[110vh] text-white overflow-hidden bg-darker">
        <div class="absolute z-10 w-full mx-auto py-30 px-0 grid grid-cols-12 bg-darker">
            <div class="absolute opacity-50 right-24 flex flex-row gap-10 py-20">
                <a href="https://instagram.com/your-account"><img src="../icons/instagram.svg" alt="ig" class="w-12"></a>
                <a href="https://instagram.com/your-account"><img src="../icons/soundcloud.svg" alt="ig" class="w-12"></a>
            </div>
            <div class="col-span-4 flex flex-col text-left gap-10 ml-24">
                <h1 class="font-primary font-bold text-6xl text-light">
                    TITLE
                </h1>
                <div class="border-b border-b-mustard w-2/3"></div>
                <p class="font-primary italic font-semibold text-lighter text-2xl">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore
                    et dolore magna aliqua.
                </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Landing',
    data() {
        return {

        }
    }
}
</script>
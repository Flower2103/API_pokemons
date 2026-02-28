<script setup>
    defineProps(['inicio','fin', 'maxlength'])
    
    const emit = defineEmits(['next','preview'])

    const activarSiguiente = () => {
        console.log('Hiciste click en siguiente');
        emit('next')
    }
</script>

<template>
    <div class ="fex justify-center space-x-4 mt-8">
        <button @click="emit('preview')"  class="rounded-full py-2 px-6 bg-red-600  hover:cursor-pointer hover:bg-red-800 text-white transition-colors shadow-2xl font-bold" :disabled="inicio <= 0">Preview {{ inicio }}</button>

        <button @click="activarSiguiente"  class="rounded-full py-2 px-6 bg-red-600  hover:cursor-pointer hover:bg-red-800 text-white transition-colors shadow-2xl font-bold" :disabled = "fin >= maxlength">Next {{ fin }} </button>
    </div>

</template>
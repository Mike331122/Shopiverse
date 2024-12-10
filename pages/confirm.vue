<template>
    <div>
        
        <h1 class="text-5xl font-extrabold dark:text-white">Please chcek your Email to confirm your registration</h1>

    </div>
</template>

<script setup>
    definePageMeta({
        middleware: [
            async (to, from) => {
                if(from.name !== 'register') return await navigateTo('/')
            }
        ]
    })
</script>
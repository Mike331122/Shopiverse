<template>
    <div class="text-center">
        <h1 class="text-5xl font-extrabold dark:text-white">Are You sure you want to logout ?</h1>
        <nuxt-link to="/">
            <button type="button" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">No, go back to home page</button>
        </nuxt-link>
        <nuxt-link to="/">
            <button @click="logout" type="button" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">Yes, I want to logout</button>
        </nuxt-link>

        <!-- Success Message --> 
        <div v-if="successMsg">
            <p id="filled_success_help" class="mt-2 text-xs text-green-600 dark:text-green-400"><span class="font-medium">Well done!</span> {{successMsg}}</p>

        </div>

        <!-- Error Messagge -->
        <div v-if="errorMsg">
            <p id="filled_error_help" class="mt-2 text-xs text-red-600 dark:text-red-400"><span class="font-medium">Oh, snapp!</span> {{errorMsg}}</p>
        </div>

        </div>
</template>

<script setup>
    const supabase = useSupabaseClient()
    const user = useSupabaseUser()
    const successMsg = useState(() => null)
    const errorMsg = useState(() => null)

    const logout = async () => {
        const {error} = await supabase.auth.signOut()
        if(error) {
            successMsg.value = null
            error.value = error.message
            return
        }

        successMsg.value = 'Hope to see You again'
        setTimeout(async () => {
            successMsg.value = null
            await navigateTo('/')
        }, 2000)
    }

    definePageMeta({
        middleware: [
            async () => {
                const user = useSupabaseUser()
                if(!user.value) await navigateTo('/login')
            }
        ]
    })
</script>
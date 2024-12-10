<template>
    <div>
        <br><br>
        <form class="max-w-lg mx-auto">
            <div>
                <div class="mb-5">
                    <label for="small-input" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Product title</label>
                    <input v-model="productTitle" type="text" id="small-input" class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                </div>

                <div class="mb-5">
                    <label for="small-input" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Product Category</label>
                    <input v-model="productCategory" type="text" id="small-input" class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                </div>

                <div class="mb-5">
                    <label for="small-input" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Product Price</label>
                    <input v-model="productPrice" type="text" id="small-input" class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                </div>

                

                <label for="message" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Product description</label>
                <textarea v-model="productDescription" id="message" rows="4" class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Write your product description..."></textarea>

                <br><br>
                <label class="block mb-2 text-sm font-medium text-gray-900 dark:text-white" for="user_avatar">Upload file</label>
                <input @change="handleFileChange" class="block w-full text-sm text-gray-900 border border-gray-300 rounded-lg cursor-pointer bg-gray-50 dark:text-gray-400 focus:outline-none dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400" aria-describedby="user_avatar_help" id="user_avatar" type="file">
                <div class="mt-1 text-sm text-gray-500 dark:text-gray-300" id="user_avatar_help">A product picture is useful to attract more buyers</div>

                <!-- Success Message --> 
                <div v-if="imageUploadSuccessMsg">
                    <p  id="filled_success_help" class="mt-2 text-xs text-green-600 dark:text-green-400"><span class="font-medium">Well done!</span> {{imageUploadSuccessMsg}}</p>

                    <br>
                </div>
                <!-- Error Messagge -->
                <div v-if="imageUploadErrorMsg">
                    <p  id="filled_error_help" class="mt-2 text-xs text-red-600 dark:text-red-400"><span class="font-medium">Oh, snapp!</span> {{imageUploadErrorMsg}}</p>

                    <br>
                </div>

                <button @click="uploadImage" type="button" class="text-white bg-gradient-to-br from-pink-500 to-orange-400 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-pink-200 dark:focus:ring-pink-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2">Upload Your image</button>

                <div class="inline-flex items-center justify-center w-full">
                    <hr class="w-64 h-1 my-8 bg-gray-200 border-0 rounded dark:bg-gray-700">
                    <div class="absolute px-4 -translate-x-1/2 bg-white left-1/2 dark:bg-gray-900">
                        <svg class="w-4 h-4 text-gray-700 dark:text-gray-300" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 18 14">
                        <path d="M6 0H2a2 2 0 0 0-2 2v4a2 2 0 0 0 2 2h4v1a3 3 0 0 1-3 3H2a1 1 0 0 0 0 2h1a5.006 5.006 0 0 0 5-5V2a2 2 0 0 0-2-2Zm10 0h-4a2 2 0 0 0-2 2v4a2 2 0 0 0 2 2h4v1a3 3 0 0 1-3 3h-1a1 1 0 0 0 0 2h1a5.006 5.006 0 0 0 5-5V2a2 2 0 0 0-2-2Z"/>
                        </svg>
                    </div>
                </div>

                <div class="text-center">

                    <button @click="createProduct" type="button" class="text-gray-900 bg-gradient-to-r from-lime-200 via-lime-400 to-lime-500 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-lime-300 dark:focus:ring-lime-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2">Submit Product</button>
    
    
                    <button @click="clearEverything" type="button" class="text-white bg-gradient-to-r from-pink-400 via-pink-500 to-pink-600 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-pink-300 dark:focus:ring-pink-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2">Celar Everything</button>
                </div>


                <!-- Success Message --> 
                <div v-if="productCreationdSuccesMsg">
                    <p id="filled_success_help" class="mt-2 text-xs text-green-600 dark:text-green-400"><span class="font-medium">Well done!</span> {{productCreationdSuccesMsg}}</p>

                </div>

                <!-- Error Messagge -->
                <div v-if="productCreationdErrorMsg">
                    <p id="filled_error_help" class="mt-2 text-xs text-red-600 dark:text-red-400"><span class="font-medium">Oh, snapp!</span> {{productCreationdErrorMsg}}</p>

                </div>

            </div>
        </form>
    </div>
</template>

<script setup>
    const supabase = useSupabaseClient()
    const user = useSupabaseUser()

    if(!user) await navigateTo('/login')

    const productTitle = useState(() => null)
    const productCategory = useState(() => null)
    const productPrice = useState(() => null)
    const productDescription = useState(() => null)
    const productImage = useState(() => null)
    const imageUrl = useState(() => null)
    const imageUploadSuccessMsg = useState(() => null)
    const imageUploadErrorMsg = useState(() => null)
    const productCreationdSuccesMsg = useState(() => null)
    const productCreationdErrorMsg = useState(() => null)

    const clearEverything = () => {
        productTitle.value = null
        productCategory.value = null
        productPrice.value = null
        productDescription.value = null
        imageUrl.value = null
        imageUploadSuccessMsg.value = null
        imageUploadErrorMsg.value = null
        productCreationdSuccesMsg.value = null
        productCreationdErrorMsg.value = null
    }

    const handleFileChange = event => {
        productImage.value = event.target.files[0]
    }

    const uploadImage = async () => {
        if(!productImage.value) {
            if(import.meta.client) {
                alert('Please select an image to upload')
            }
            return
        }

        const image = productImage.value
        try {
            const {data, error} = await supabase.storage.from('images').upload(`public/${image.name}`, image, {
                cacheControl: '3600',
                upsert: false
            })

            if(data){
                imageUploadSuccessMsg.value = 'Image Uploaded'
                const {data} = await supabase.storage.from('images').getPublicUrl(`public/${image.name}`)
                imageUrl.value = data.publicUrl
            }
        } catch(err) {
            imageUploadErrorMsg.value = err.message
        }
    }

    const createProduct = async () => {
        const {data: product, error} = await useFetch('/api/products/create-new-product', {
            query: {
                title: productTitle,
                description: productDescription,
                image: imageUrl,
                category: productCategory,
                price: productPrice
            }
        })

        if(error.value) {
            productCreationdErrorMsg.value = 'An error happend, try again...'
            return
        }

        productCreationdSuccesMsg.value = 'Product created auccesfully, Redirecting...'
        const productID = product.value.id
        setTimeout(async () => {
            await navigateTo(`/product-${productID}`)
        },2000)
    }
</script>
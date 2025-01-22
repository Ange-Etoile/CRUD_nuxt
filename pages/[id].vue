<script setup>
import { ref,reactive,onMounted } from 'vue';
import axios from 'axios'
import { useRoute } from 'vue-router';
const route = useRoute();
    useHead({
        title:'Home Page'
    })
    const post = ref({})
    let isLoading = ref(false)
    let errors = ref({})
    const postId = route.params.id
    function getStudent(studentId){
        isLoading = true
        axios.get(`https://jsonplaceholder.typicode.com/posts/${postId}`).then(res =>{
            isLoading = false
            post = res.data.post
        })

    }
function EditPost() {

    isLoading = true
    axios.get(`https://jsonplaceholder.typicode.com/posts/${postId}`).then(res =>{
            isLoading = false
            post = res.data.post
            console.log(post)
        })
    // axios.put(`https://jsonplaceholder.typicode.com/posts/${postId}`,post).then(res=>{
    //     alert(res)
    //     isLoading = false
    //    errors = {}
    // })
    // .catch(function(error){
    //     console.error(error)
    //         // errors = error.response.data.errors 
        
    // })

  
}
</script>
<template>
        <div class="mt-5 container w-full">
        <div class="p-2  flex flex-col justify-center gap-4 w-full">
            <div class="">
                <h4>Edit Post <NuxtLink to="/students/index" class="rounded-lg bg-red-600 float-end px-4 py-2">Back</NuxtLink></h4>
            </div>
            <div class="flex flex-col gap-6 justify-center items-center w-[50%] p-4">
                <div v-if="isLoading">
                    <span>Loading ...</span>
                </div>
                <div v-else>
                    <form @submit.prevent="EditPost()"  class="w-full">
                    <div class="mb-3 flex justify-between">
                        <label for="">userId :</label>
                        <input v-model.number="post.userId" type="text" class="rounded-lg p-2 w-[70%]  border-black border-2">
                    </div>
                    <div class="mb-3 flex justify-between">
                        <label for="">Title:</label>
                        <input v-model="post.title" type="text" class="rounded-lg p-2 w-[70%]  border-black border-2">
                    </div>
                    <div class="mb-3 flex justify-between">
                        <label for="">Body:</label>
                        <input v-model="post.body" type="text" class="rounded-lg p-2 w-[70%]  border-black border-2">
                    </div>
                    <div class="mb-3">
                        <button type="submit" class="rounded px-4 py-2 bg-green-500">Update</button>
                    </div>
                </form>
                </div>
            </div>
        </div>
    </div>
</template>
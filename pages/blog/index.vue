<script setup>
import { useFetch } from 'nuxt/app';

    const {data: posts} = useFetch('https://jsonplaceholder.typicode.com/posts?_limit=10')
    
    function deleteStudent(studentId){
        if(confirm('Are you sure, you want to delete this data ?')){
            axios.delete(`https://jsonplaceholder.typicode.com/posts/${studentId}`).then(res =>{
                getStudents()

            })
        }
    }
</script>
<template>
    <div class="flex flex-col justify-center items-center w-[80%] gap-4">
        <div class="flex justify-between w-full">
            <h4>Student Lists <NuxtLink to="/create" class="rounded-lg bg-blue-600 float-end px-4 py-2">Add Student</NuxtLink></h4>
        </div>
        <div v-if="isLoading">
            <span>Loading...</span>
        </div>
        <div>
            <table class="table">
                <thead>
                    <tr>
                        <th>Id</th>
                        <th>UserId</th>
                        <th>Title</th>
                        <th>Body</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(post, index) in posts " :key="index">
                        <td>{{ post.id }}</td>
                        <td>{{ post.userId }}</td>
                        <td>{{ post.title }}</td>
                        <td>{{ post.body }}</td>
                        <td>
                            <NuxtLink :to="`/${post.id}`" class="rounded-lg bg-green-500 mx-2 px-4 py-2">Edit</NuxtLink>
                            <button type="button" @click="deleteStudent(post.id)" class="rounded-lg bg-red-500 mx-2 px-4 py-2">Delete</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
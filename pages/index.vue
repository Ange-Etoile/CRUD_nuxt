<script>
   
   
import { deleteProject, getProjects } from '~/services/projectService'
import Swal from 'sweetalert2'
 
export default {
 
  data() {
    return {
      projects:[]
    };
  },
   
  created(){
    this.fetchProjectList()
  },
 
  methods:{
    fetchProjectList() {       
      getProjects()
        .then(response => {
            this.projects = response.data;
            return response
        })
        .catch(error => {
          return error
      });
 
    },
 
    handleDelete(id){
      Swal.fire({
          title: 'Are you sure?',
          text: "You won't be able to revert this!",
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes, delete it!'
        }).then((result) => {
        if (result.isConfirmed) {
          deleteProject(id)
            .then( response => {
                Swal.fire({
                    icon: 'success',
                    title: 'Project deleted successfully!',
                    showConfirmButton: false,
                    timer: 1500
                })
                this.fetchProjectList();
                return response
            })
            .catch(error => {
                Swal.fire({
                      icon: 'error',
                    title: 'An Error Occured!',
                    showConfirmButton: false,
                    timer: 1500
                })
                return error
            });
        }
      })
    }
   
 
  }
};
</script>

<template>
    <div class="container mx-auto">
        <h2 class="text-center font-bold text-2xl mt-10 mb-4">Project Manager</h2>
        <div class="bg-white  shadow-md rounded-lg overflow-hidden">
            <div class="bg-gray-100 p-4 rounded-t-lg flex justify-between">
            <NuxtLink to="/create" class="border rounded-md px-5 py-2.5 text-base font-medium text-indigo-600 hover:bg-indigo-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">Create New Project</NuxtLink>
            </div>
            <div class="p-4 flex justify-center items-center rounded-lg">
                <table class="border  border-collapse border-gray-200 rounded-lg">
                    <thead>
                    <tr>
                        <th class="p-3 text-left font-medium text-gray-900 border border-b border-gray-200 bg-gray-100">Name</th>
                        <th class="p-3 text-left font-medium text-gray-900 border border-b border-gray-200 bg-gray-100">Description</th>
                        <th class="p-3 text-left font-medium text-gray-900 border border-b border-gray-200 bg-gray-100" width="240px">Action</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="project in projects" :key="project.id">
                        <td class="p-3 border border-b border-gray-200">{{ project.name }}</td>
                        <td class="p-3 border border-b border-gray-200">{{ project.description }}</td>
                        <td class="p-3 border border-b border-gray-200">
                        <NuxtLink :to="`/edit/${project.id}`" class="border rounded-md px-5 py-2.5 text-base font-medium text-green-600 hover:bg-green-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500">Edit</NuxtLink>
                        <button @click="handleDelete(project.id)" class="border rounded-md px-5 py-2.5 text-base font-medium text-red-600 hover:bg-red-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500">Delete</button>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>
    

  
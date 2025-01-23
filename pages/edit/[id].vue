<script>
   
  import { NuxtLink } from '#components';
  import Swal from 'sweetalert2'
  import { getProject, updateProject } from '~/services/projectService';
   
  export default {
   
    data() {
      return {
        project: {
          name: '',
          description: '',
        },
        isSaving:false,
      };
    },
    created() {
      const id = this.$route.params.id;
      getProject(id)
      .then(response => {
          let projectInfo = response.data
          this.project.name = projectInfo.name
          this.project.description = projectInfo.description
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
      })
    },
    methods: {
      handleSave() {
          this.isSaving = true
          const id = this.$route.params.id;
   
          updateProject(id, this.project)
            .then(response => {
              Swal.fire({
                  icon: 'success',
                  title: 'Project updated successfully!',
                  showConfirmButton: false,
                  timer: 1500
              })
              this.isSaving = false
              this.project.name = ""
              this.project.description = ""
              return response
            })
            .catch(error => {
              this.isSaving = false
              Swal.fire({
                  icon: 'error',
                  title: 'An Error Occured!',
                  showConfirmButton: false,
                  timer: 1500
              })
              return error
            });
      },
    },
  };
</script>
<template>
    <h2 class="text-center font-bold text-2xl mt-10 mb-4">Edit Project</h2>
    <div class="bg-white shadow-md rounded-lg overflow-hidden">
        <div class="bg-gray-100 p-4 rounded-t-lg flex justify-end">
            <NuxtLink to="/" class="border rounded-md px-5 py-2.5 text-base font-medium text-info-600 hover:bg-info-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-info-500">View All Projects</NuxtLink>
        </div>
        <div class="p-4">
            <form @submit.prevent="handleSave">
            <div class="mb-4">
                <label for="name" class="block text-gray-700 font-bold mb-2">Name</label>
                <input v-model="project.name" type="text" class="shadow appearance-none border rounded-md w-full py-2 px-3 text-gray-700 focus:outline-none focus:ring-2 focus:ring-indigo-600 focus:ring-opacity-50" id="name" name="name" required>
            </div>
            <div class="mb-4">
                <label for="description" class="block text-gray-700 font-bold mb-2">Description</label>
                <textarea v-model="project.description" class="shadow appearance-none border rounded-md w-full py-2 px-3 text-gray-700 focus:outline-none focus:ring-2 focus:ring-indigo-600 focus:ring-opacity-50" id="description" rows="3" name="description" required></textarea>
            </div>
            <button type="submit" :disabled="isSaving" class="disabled:opacity-50 border rounded-md px-5 py-2.5 text-base font-medium text-indigo-600 hover:bg-indigo-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                Save Project
            </button>
            </form>
        </div>
    </div>
</template>
      
  
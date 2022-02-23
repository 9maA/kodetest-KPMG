<template>
  <div class="container">
      <h1 class = "text-center mt-5">Review notes</h1>

      <!-- Input -->
      <div class="d-flex">
          <input v-model="task" type="text" placeholder="Enter task" class="form-control">
          <button @click="submitTask" class="btn btn-secondary rounded-0">SUBMIT</button>
      </div>

      <!-- Task table -->
      <table class="table table-bordered mt-5">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col">Priority</th>
            <th scope="col" class="text-center">#</th>
            <th scope="col" class="text-center">#</th>
          </tr>
          <tr>
            <!-- OBS! Rakk ikke å implementere searchbaren -->
            <th scope="col" colspan="5"><input class="form-control mr-sm-2" type="search" placeholder="Search for tasks..." aria-label="Search"></th> 
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in computedTask" :key="index"> 
            <td>
              <span :class="{'finished': task.status === 'finished'}">{{task.title}}</span> 
            </td>
            <td style="width: 200px">
              <span @click="changeStatus(index)" class="pointer" 
                :class="{'text-secondary': task.status === 'Not started', 
                'text-primary': task.status === 'In Progress', 
                'text-warning': task.status === 'Pending documentation',
                'text-info': task.status === 'Addressed',
                'text-danger': task.status === 'Closed'}"
              >
                {{task.status}}
              </span>
            </td>
            <td style="width: 120px">
              <span @click="changePriority(index)" class="pointer" 
                :class="{'text-danger': task.priority['text'] === 'High', 
                'text-warning': task.priority['text'] === 'Medium', 
                'text-success': task.priority['text'] === 'Low'}"
              >
                {{task.priority['text']}}
              </span>
            </td>
            <td>
              <div class="text-center" @click="editTask(index)">
                <span class="fa fa-pen"></span>
              </div>
            </td>
            <td>
              <div class="text-center" @click="deleteTask(index)">
                <span class="fa fa-trash"></span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    
      <div class="text-center"> 
        <button @click="limit = null" type="button" class="btn btn-primary btn-sm"><span class="fa fa-arrow-down"></span> Load More...</button>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {

  data(){
    return{
      task: '',
      editedTask: null,
      availableStatuses: ['Not started', 'In Progress', 'Pending documentation', 'Addressed'],
      availablePriorities: ['High', 'Medium', 'Low'],
      tasks: [],
      limit: 5,

      tasksStatic: [ //dummystatistics som ble brukt under testing
        {
          title: 'Administrative work',
          status: 'Not started',
          priority: 'Medium'
        },
        {
          title: '',
          status: 'Pending documentation',
          priority: 'Low'
        }
      ],
    }
  },

  async created(){
    axios.get("http://localhost:3000/data").then(response =>{
      this.tasks = response.data;
      console.log(response.data)
    })
    .catch(error => {
      console.log(error);
    })
    
  },

  computed:{
    computedTask(){
      return this.limit ? this.tasks.slice(0,this.limit) : this.tasks
    }
  },   


  methods: {

    submitTask(){ //statisk innsetting; rakk ikke å bruke post-request for å lagre det i backenden
      if(this.task.length == 0) return;

      if(this.editedTask == null){
        this.tasks.push({
          title: this.task,
          status: 'Not started',
          priority: 'Low'
        });
      }
      else{
        this.tasks[this.editedTask].title = this.task;
        this.editedTask = null;
      }

      this.task = '';
    },

    deleteTask(index){ //statisk innsetting; rakk ikke å bruke post-request for å lagre det i backenden
      this.tasks.splice(index, 1);
    },

    editTask(index){ //statisk innsetting; rakk ikke å bruke post-request for å lagre det i backenden
      this.task = this.tasks[index].title
      this.editedTask = index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 3) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    changePriority(index){
      let newIndex = this.availablePriorities.indexOf(this.tasks[index].priority['text']);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].priority['text'] = this.availablePriorities[newIndex];
    }
  }
}
</script>

<style scoped>

.pointer{
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
.d-flex{
  margin-bottom: 30px;
}

</style>
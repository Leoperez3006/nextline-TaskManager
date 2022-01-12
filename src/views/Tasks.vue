<template>
  <v-container>
    <div class="col">
      <div class="row addTask">
        <!-- <v-btn
          elevation="2"
          @click="tryPost()"
        > post</v-btn> -->
        <addTask :addTaskFunc ="addTask"></addTask>
      </div>
      <div class="row">
        <v-list class="overflow-y-auto list" max-height="600" >
          <template >
            <Task 
              v-for="(t,i) in tasks" :key="i"
              :id="t.id"
              :title="t.title" 
              :date="t.due_date" 
              :completed="t.is_completed"
              :deleteTask = "deleteTask"
              :editTaskFunc = "editTaskFunc"
              :completeTask = "completeTask"
            ></Task>
          </template>
        </v-list>
      </div>
    </div>
  </v-container> 
</template>

<script>
import { config } from '../utils/apiData'
import formurlencoded from 'form-urlencoded'
import Task from '../components/Task.vue' 
import addTask from '../components/AddTask.vue'
import axios from 'axios'
  export default {
    name: 'Tasks',
    components: {
      Task,
      addTask
    },
    data () {
      return {
        tasks: [],
        modalShowMore: false
      }
    },
    methods: {
      // this method retrieves the tasks from the API
      getTasks () {
        axios.get('https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks?token=untokenxd',config)
        .then ( (response) => {  
          console.log(response.data)
          this.tasks = response.data
        })
        .catch (function (error) {
          // console.log(error)
        })
      },
      // this method call the API to delete the task
      deleteTask(id) {
        console.log('delete task: ', id)
        axios.delete('https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/' + id +'?token=untokenxd',config)
        .then( (response) => {
          // console.log('deleted')
          this.tasks = []
          this.getTasks()
        })
        .catch ( function (error) {
          // console.log('Not deleted')
        })
      },
      // this method calls the API to update a task's data
      editTaskFunc(task){
        console.log(task)
        // console.log('edit Task:', id)
      },
      // this method changes the status of the task to completed
      completeTask(id){
        console.log(id)
      },
      //this method calls the API to upload a new task
      addTask(x){
        // console.log('add task: ', x)
        // console.log('params', params)
        axios.post('https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',
        {
          params: {
            title: 'titulo',
            token: 'untokenxd'
          }
        }
        ,config)
        .then( (response) => {
          console.log(response)
        })
        .catch( function (error) {
          // console.log(error)
        })
      },
      tryPost(){
        let data = {
          title: 'titulo',
          token: 'untokenxd'
        }
        console.log(formurlencoded(data))
        axios.post('https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',
        {
          params: formurlencoded(data)
        }
        ,config)
        .then( (response) => {
          console.log(response)
        })
        .catch( function (error) {
          // console.log(error)
        })
      }
    },
    created () {
      // this method is called when the  view is created 
      this.getTasks()
      // console.log('mounted de tasks')
    }
  }
</script>

<style scoped>
.list{
  width: 100%;
  background: rgba(0, 0, 0, 0);
}

.addTask{
  margin: 5px;
  align-items: end;
}
</style>

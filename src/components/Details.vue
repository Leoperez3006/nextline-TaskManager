<template>
    <div>
      <v-btn
          icon
          color="primary"
          @click="retrieveTaskData(id)"
      >
      <v-icon>mdi-dots-horizontal</v-icon>
      </v-btn>
      <v-dialog
      transition="dialog-top-transition"
        v-model="dialog"
        max-width="600"
      >
        <v-card>
          <v-card-title class="text-h5">
            {{ taskData.title }}
          </v-card-title>
          <v-divider class="mx-5"></v-divider>
          <v-card-text>
            <div class="col">
                  <div class="row">
                    <strong> Due Date: </strong> {{taskData.due_date}}
                </div>
                <div class="row">
                    <strong> Description: </strong> {{taskData.description}}
                </div>
                  <div class="row">
                    <strong> Comments: </strong> {{taskData.comments}}
                </div>
                <div class="row">
                            <v-divider ></v-divider>
                </div>
                <div class="row">
                    <strong> Tags:</strong>
                </div>
                <div class="row">
                  <v-chip
                  class="tags"
                  v-for="(tag, index) in tagsArray"
                  :key="index"
                  draggable>
                      {{tag}}
                  </v-chip>
                </div>
                <div class="row">
                </div>
            </div>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="primary"
              text
              @click="dialog = false"
            >
              close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
</template>

<script>
import axios from 'axios'
import { config } from '../utils/apiData'
export default{
    name: 'TaskDetails',
    props:{
        id:{
            type: Number
        }
    },
    data() {
        return{
            dialog: false,
            taskData: {
            },
            tagsArray: []
        }
    },
    methods: {
        // this metod retrieves the task's aditional info to put it in a dialog
        retrieveTaskData(x){
            // console.log('Activar modal con id: ', x)
            axios.get('https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/' + this.$props.id + '?token=untokenxd',config)
            .then ( (response) => {  
                // console.log(response.data)
                this.taskData = response.data[0]
                // console.log('taskdata title:', this.taskData.title)
                this.tagsArray = response.data[0].tags.split(',')
                this.dialog = true
            })
            .catch (function (error) {
            // console.log(error)
            })
        },
        },
        mounted () {
        }
    }
</script>

<style scoped>
.tags{
  margin: 2px;
  color: whitesmoke;
}
</style>
<template>
    <v-container class="myTask" :style="{ background: computedColor }" >
        <v-banner
        single-line
        >
            <v-list-item-action>
                <v-checkbox v-if="completed"  disabled v-model="checkbox" @click="toggleTask()"></v-checkbox>
                <v-checkbox v-else   v-model="checkbox" @click="toggleTask(id)"></v-checkbox>
            </v-list-item-action>
            {{ tit}}
            <v-list-item-action class = "date">
                {{dat}}
            </v-list-item-action>
            <template v-slot:actions >
                <TaskDetails :id="id"></TaskDetails>
                <EditTask :id="id" :editTaskFunc="editTaskFunc" > </EditTask>
                <v-btn
                    icon
                    color="red"
                    @click="deleteTask(id)"
                >
                    <v-icon dark>
                        mdi-delete
                    </v-icon>
                </v-btn>
            </template>
        </v-banner>
    </v-container>
</template>
<script>
import taskDetails from './Details.vue'
import EditTask from './EditTask.vue'
export default {
    name: 'Task',
    components: {
        taskDetails,
        EditTask
    },
    data () {
        return{
        color: '#f1b76e',
        checkbox: this.$props.completed == 0 || this.$props.completed === null ? false : true ,
        tit: this.$props.title != null || this.$props.tittle === '' ? this.$props.title : 'No title given',
        dat: this.$props.date != null || this.$props.date === '' ? this.$props.date : 'No date available'
        }
    },
    props: {
        id: {
            type: Number
        },
        title: {
            type: String,
            default: 'Task name'
        },
        completed : {
            type: Number,
            default: 0
        },
        date: {
            type: String,
            default: 'date not available'
        },
        deleteTask: {
            type: Function,
        },
        editTaskFunc: {
            type: Function
        },
        completeTask : {
            type: Function
        }
    },
    methods: {
        // this is a method that changes de color of the task if it is completed
        changeColor() {
            this.color = this.checkbox === false || this.checkbox === null ? this.color : '#b4c5d5'
        },
        toggleTask(id) {
            // console.log('togle task with id:', this.$props.id)
            this.completeTask(id)
        }
    },
    computed: {
      // this a computed function returns the color for the styling of the card
        computedColor: function () {
            return this.color
        }
    },
    created () {
        // this method is called when the component is created to change the color on the right time
        this.changeColor()
        // console.log('props: ', this.$props)
    }
}
</script>

<style scoped>
.myTask{
  /* background: rgb(71, 141, 89); */
    width: 100%;
    font-size: 1.5rem;
    margin-top: 10px;
    border-radius: 10px;
    box-shadow: 4px 4px 3px 1px #c5c4c4c4;
}
.date{
    font-size: 12px;
    color: whitesmoke;
}

</style>
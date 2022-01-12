<template>
    <v-row class="d-flex align-end flex-column">
        <v-dialog
            v-model="dialog"
            fullscreen
            hide-overlay
            transition="dialog-bottom-transition"
        >
        <template v-slot:activator="{ on, attrs }">
        <v-btn
            icon
            color="primary"
            v-bind="attrs"
            v-on="on"
            @click="edit"
        >
        <v-icon>mdi-pencil</v-icon>
        </v-btn>
        </template>
        <v-card>
            <v-toolbar
            dark
            color="#405664"
            >
            <v-btn
                icon
                dark
                @click="dialog = false"
            >
                <v-icon>mdi-close</v-icon>
            </v-btn>
            <v-toolbar-title>Edit this task</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-toolbar-items>
                <v-btn
                    dark
                    text
                    :disabled="!valid"
                    @click="validate"
                >
                Save
                </v-btn>
            </v-toolbar-items>
            </v-toolbar>
            <v-subheader>task details</v-subheader>
            <v-divider></v-divider>
                <v-list class="form">
                    <form ref = "form"
                        v-model="valid"
                        lazy-validation
                    >
                        <v-text-field
                            v-model="addTask.title"
                            outlined
                            :rules = "titleRules"
                            label="Title"
                            required
                        ></v-text-field>
                        <v-text-field
                            v-model="addTask.description"
                            label="Description"
                            outlined
                        ></v-text-field>
                        <v-textarea
                            v-model="addTask.comments"
                            label="Comments of this task"
                            outlined
                            auto-grow
                            rows="4"
                            row-height="25"
                        ></v-textarea>
                        <v-textarea
                            v-model="addTask.tags"
                            label="Tags"
                            outlined
                            auto-grow
                            rows="4"
                            row-height="25"
                            placeholder='add tags separated by " , "'
                        ></v-textarea>
                        <v-menu
                            ref="menu"
                            v-model="menu"
                            :close-on-content-click="false"
                            :return-value.sync="addTask.due_date"
                            transition="scale-transition"
                            offset-y
                            min-width="auto"
                        >
                            <template v-slot:activator="{ on, attrs }">
                            <v-text-field
                                v-model="addTask.due_date"
                                label="Due date"
                                prepend-icon="mdi-calendar"
                                readonly
                                v-bind="attrs"
                                v-on="on"
                            ></v-text-field>
                            </template>
                            <v-date-picker
                            v-model="addTask.due_date"
                            no-title
                            scrollable
                            >
                            <v-spacer></v-spacer>
                            <v-btn
                                text
                                color="primary"
                                @click="menu = false"
                            >
                                Cancel
                            </v-btn>
                            <v-btn
                                text
                                color="primary"
                                @click="$refs.menu.save(addTask.due_date)"
                            >
                                OK
                            </v-btn>
                            </v-date-picker>
                        </v-menu>
                    </form>
                </v-list>
        </v-card>
        </v-dialog>
        <v-snackbar
            v-model="snackbar"
            color="#d37a50"
            :timeout="timeout"
        >
        A title is required to save the task
        </v-snackbar>
    </v-row>
    </template>

<script>
    import axios from 'axios'
    import { config } from '../utils/apiData'
export default {
    name: 'EditTask',
        props:{
            editTaskFunc: {
            type: Function
            },
            id: {
                type: Number
            }
        },
    data () {
        return {
            titleRules: [
            v => !!v || 'Title is required',
            ],
            snackbar: false,
            timeout: 1000,
            valid: true,
            menu: false,
            dialog: false,
            notifications: false,
            sound: true,
            widgets: false,
            addTask: {
                title: null,
                is_completed:  null,
                due_date: null,
                comments: null,
                description: null,
                tags: null,
                token: 'untokenxd',
            }
        }
    },
    watch: {
    },
    methods: {
        edit() {
            axios.get('https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/' + this.$props.id + '?token=untokenxd',config)
            .then ( (response) => {  
                console.log(response.data[0])
                // console.log('taskdata title:', this.taskData.title)
                this.addTask =  response.data[0]
                this.dialog = true
            })
            .catch (function (error) {
            // console.log(error)
            })
        },
    // this method validates if the title is valid, if not activates snackbar
        validate () {
            if (this.addTask.title != null){
                this.updateTask(task)
                this.dialog = false
                this.addTask = {}
            } else { 
                this.snackbar = true;
            }
        }
    },
    created () {
    },
    mounted () {
    }
}
</script>
<style scoped>
.form{
    padding-left: 10%;
    padding-right: 10%;
    padding-top: 30px;

}
</style>
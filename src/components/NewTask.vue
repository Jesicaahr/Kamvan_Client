<template>
    <div class="newKanban">
        <!-- Button trigger modal -->
        <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
            Add Task
        </button>
         
        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Add New Task</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
                </div>
                <div class="modal-body">
                    <form  @submit.prevent="createTask">
                        <div class="form-group">
                        <label for="recipient-name" class="col-form-label">Title:</label>
                        <input type="text" class="form-control" id="task-title" v-model="task.title" required>
                        </div>
                        <div class="form-group">
                        <label for="message-text" class="col-form-label">Description:</label>
                        <textarea class="form-control" id="task-assigned" v-model="task.description"></textarea>
                        </div>
                        <div class="form-group">
                        <label for="message-text" class="col-form-label">Assigned To:</label>
                        <textarea class="form-control" id="task-assigned" v-model="task.assignedTo"></textarea>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                <button type="button" class="btn btn-primary" data-dismiss="modal" aria-label="Close" @click.prevent="createTask">Create</button>
                </div>
            </div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios'

export default {
    name: 'NewTask',
    components: {

    },
    data(){
        return {
            task: {
                title: '',
                description: '',
                assignedTo:'',
            }
        }
    },
    methods: {
        createTask(){
            let title = this.task.title
            let description = this.task.description
            let assignedTo = this.task.assignedTo

            axios({
                method: 'post',
                url: 'https://fierce-reef-02367.herokuapp.com/tasks',
                headers : {
                    access_token: localStorage.token
                },
                data: {
                    title,
                    description,
                    assignedTo
                }
            })
                .then(({data}) => {
                    this.task.title=''
                    this.task.description=''
                    this.task.assignedTo=''
                    swal("Yashhh", data.msg, "success");
                    this.$emit('refetchTasks')
                })
                 .catch(err => {
                    swal("Whopss", err.response.data.errors[0].message, "error");
                })
        }
    }

}
</script>

<style>

</style>
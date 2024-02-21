<template>
    <div class="container" style="max-width: 600px;">

        <!-- Heading start -->
        <h2 class="text-center mt-5">My Vue Todo App</h2>

        <!-- input -->
        <div class="d-flex mt-5">
            <input 
                type="text" 
                placeholder="Input Task" 
                v-model="task" 
                class="w-75 form-control"
            />
            <button @click="submitTask()" class="btn btn-warning rounded">SUBMIT</button>
        </div>

        <!-- Task Table -->
        <table class="table table-bordered mt-5">
            <thead>
                <tr>
                <th scope="col">Task</th>
                <th scope="col" style="width: 120px;">Status</th>
                <th scope="col" class="text-center">Edit</th>
                <th scope="col" class="text-center">Delete</th>
                </tr>

                <tr v-for="(task, index) in tasks" :key="index">
                    <td>
                        <span :class="{'finished' : task.status === 'finished'}">
                            {{ task.name }}
                        </span>
                    </td>
                    <td>
                        <span @click="changeStatus(index)" class="pointer"
                            :class="{'text-danger' : task.status === 'to-do',
                            'text-warning' : task.status === 'in-progress'
                            }"
                           
                        >
                            {{firstCharUpper(task.status)}}
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
            </thead>

            <tbody>
            </tbody>

        </table>

    </div>
</template>

<script>
    export default {
        name: 'TodoApp',
        editedTask: null,
        availableStatuses: ['to-do', 'in-progress', 'finished'],
        data() {
            return{
                task: '',
                tasks: [
                    {
                        name: 'Steal bananas from the store',
                        status: 'to-do'
                    },
                    {
                        name: 'Message with girl friend',
                        status: 'in-progress'
                    }
                ]
            }
        },
        methods: {
            submitTask() {
                if(this.task.length === 0) return;
                
                if (this.editedTask != null) {
                    this.tasks[this.editedTask].name = this.task;
                    this.editedTask = null;
                } else {
                /* We need to add new task */
                    this.tasks.push({
                    name: this.task,
                    status: "to-do",
                    });
                }
                this.task = '';             
            },
            deleteTask(index) {
                this.tasks.splice(index, 1);
            },
            editTask(index) {
                this.task = this.tasks[index].name;
                this.editedTask = index;
            },
            changeStatus(index) {
                let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
                if (++newIndex > 2) newIndex = 0;
                this.tasks[index].status = this.availableStatuses[newIndex];
            },
            firstCharUpper(str) {
                return str.charAt(0).toUpperCase() + str.slice(1);
            }
        }
    }
</script>

<style scoped>
.pointer {
    cursor: pointer;
}
.finished {
    text-decoration: line-through;
}
</style>
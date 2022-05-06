<template>
    <div class="tasks_container">
        <input type="checkbox" name="" id="">
        <div class="add_task" >

            <form v-on:submit.prevent="submitForm">
                <div class="form-group">
                    <label for="title">Title</label>
                    <input type="text" class="form-control" id="title" v-model="title">
                    <input type="text" name="" id="">
                </div>
                <div>
                    <label for="description">Description</label>
                    <textarea class="form-control" id="description" v-model="description"></textarea>

                </div>
                <div class="form-group">
                    <button @click="submitForm" type="submit">Add Task</button>

                </div>
            </form>
        </div>
        <div class="tasks_content">
            <h1>Tasks</h1>
            <ul class="tasks_lists">
                <li v-for="task in tasks" :key="task.id">
                    <h2>{{ task.title }}</h2>
                    <p>{{ task.description }}</p>
                    <button @click="toggleTask(task)">
                        {{ task.completed ? 'Undo' :'Complete' }}
                    </button>
                    <button @click="deleteTask(task)" style="color: red; font-size: 15px;">Delete</button>

                </li>

            </ul>

        </div>
    </div>
</template>




<script>
import axios from 'axios'
export default {
    name: 'TasksComponent',
    
    data() {
        return{
            tasks: [],
            title: '',
            description: '',
        }
        // mounted() {

        // }


    },
    methods: {
        async getData() {
            axios.get('http://localhost:8000/api/tasks/')
            .then(res => this.tasks=res.data)
            .catch(error => console.log(error))
            // try{
            //     const response = await this.$http.get('http://localhost:8000/api/tasks/');
            //     this.tasks = response.data;
            // } catch (error){
            //     console.log(error);
            // }
        },
        created() {
            this.getData();
        },

        async submitForm() {
            // const {title, description, completed} = addTask;
            axios.post('http://localhost:8000/api/tasks/',{
                title: this.title,
                description: this.description,
                completed: false,
            })
            .then(res => this.tasks = [...this.tasks, res.data])
            .catch(err => console.log(err))
            // try {
            //     const response = await this.$http.post('http://localhost:8000/api/tasks/', {
            //         title: this.title,
            //         description: this.description,
            //         completed: false,
            //     });
            //     this.tasks.push(response.data);
            //     this.title = '';
            //     this.description = '';
            // }
            //  catch (error) {
            //     console.log(error);
            // }
        },
        async toggleTask(task) {
            try {
                const response = await this.$http.put(`http://localhost:8000/api/tasks/${task.id}`,{
                    completed: !task.completed,
                    title: task.title,
                    description: task.description
                });
                let taskIndex = this.tasks.findIndex(t => t.id ===  task.id);
                this.tasks = this.tasks.map((task) => {
                    if(this.tasks.findIndex(t => t.id === taskIndex)=== taskIndex ){
                        return response.data;
                    }
                    return task;
                });
            } catch (error){
                console.log(error);
            }

        },

        async deleteTask(task) {
            let confirmation = confirm('Do you want to delete this');
            if(confirmation){
                try{
                    await this.$http.delete(`http://localhost:8000/api/tasks/${task.id}`);
                    this.getData();
                }
                catch(error){
                    console.log(error)
                }
            }
        },
        
    },

    created() {
        this.getData();
    }


    
}
</script>





<style scoped>
.form-group {
    color: red;
    font-weight: bold;
}
</style>
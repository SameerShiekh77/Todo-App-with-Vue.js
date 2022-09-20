<template>
    <div>
        <h1>Todo App</h1>

        <form @submit="submitData">
            <input required v-model="tasks.task" type="text" placeholder="Enter your Task"> <br><br>
            <input required v-model="tasks.status" type="radio" name="status" value="pending"> Pending 
            <input required v-model="tasks.status" type="radio" name="status" value="completed"> Completed <br><br>
            <button>Add Task</button>
        </form>
        <br><br>
<div>
    <table border="1px">
        <tr>
            <th>Task</th>
            <th>Status</th>
            <th colspan="2">Actions</th>
        </tr>

        <tr v-for="item in list" :key="item.id">
            <td>{{item.task}}</td>
            <td>{{item.status}}</td>
            <td><button @click="updateData(item.id)">Update</button></td>
            <td><button @click="deleteData(item.id)">Delete</button></td>
        </tr>
    </table>
</div>
    </div>
</template>


<script>
import axios from 'axios';
export default{
    name: 'Todo',
    data(){
        return{
            tasks:{

                task: null,
                status: null,
            },
            list: null

        }
    },
    methods:{
        getData(){
            axios.get('http://localhost:3000/posts/')
            .then(resp=>{
                this.list = resp.data
            })
        },
        submitData(e)
        {
            axios.post("http://localhost:3000/posts", this.tasks)
            .then((resp)=>{
                console.log(resp.data);
                this.getData();
                
            })
            this.tasks.username = "";
                this.tasks.task = "";
                this.tasks.status = "";
            e.preventDefault();
        },
        updateData(id){
            axios.get('http://localhost:3000/posts/'+id)
            .then(resp=>{
                this.tasks.status= resp.data.status
                this.tasks.task = resp.data.task
            })
        },
     

        deleteData(id)
        {
            axios.delete('http://localhost:3000/posts/'+id)
            .then(rep=>{
                alert("Task has Deleted");
                this.getData();
            })
        }
    },
    mounted()
    {
        this.getData();
    }

}
</script>
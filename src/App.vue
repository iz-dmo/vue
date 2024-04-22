<template>
  <div class="">
    <h1 class="text-white bg-primary p-3 text-center">{{ title }}'s To Do List</h1>
    <div class="container">
      <div class="row d-flex justify-content-end mb-3">
        <div class="col-5"><input type="text" placeholder="Enter your to do lists" class="form-control" v-on:keyup.enter="add_tasks()" v-model="new_task"></div>
        <div class="col-2"><button class="btn btn-success" @click="add_tasks()">ADD</button></div>
        <div class="col-2"><button class="btn btn-danger" @click="delete_tasks()">Delete Tasks</button></div>
      </div>
     <div class="" v-if="filter_task.length > 0">
      <div class="row mb-4">
        <div class="col fs-3 fw-bold">Task</div>
        <div class="col-2 fs-3 fw-bold">Done</div>
      </div>
      <div class="row" v-for="(task,index) in filter_task" :key="index">
        <div class="col" :class="task.done ? 'delete' : ''">{{ task.action }}</div>
        <div class="col-2">
          <input type="checkbox" v-model="task.done">
        </div>
      </div>
      <div class="bg-danger text-white p-2 mt-3 d-flex justify-content-center align-items-center">
        <div class="row">
          <div class="col">Hide Completed Tasks</div>
          <div class="col-2"><input type="checkbox" v-model="hideCompleted"></div>
        </div>
      </div>
     </div>
      <div class="alert alert-warning text-center" v-else>Empty Data!</div>

    </div>
  </div>
  
</template>

<script>
  export default {
    name : "App",
    data : () => ({
      title : "Dmo",
      hideCompleted : false,
      new_task : '',
      tasks : []
    }),
    computed : {
      filter_task(){
        return this.hideCompleted ? this.tasks.filter((v) => !v.done) : this.tasks;
      }
    },
    methods : {
      add_tasks(){
        if(this.new_task == ''){
          return alert("please enter something!");
        }
        this.tasks.push({
          action : this.new_task,
          done : false,
        });
        this.storeData();
        this.new_task = '';
      },
      delete_tasks(){
        this.tasks = this.tasks.filter((v)=> !v.done);
        this.storeData();
      },
      storeData(){
        localStorage.setItem('myLocalTask',JSON.stringify(this.tasks));
      }
    },
    mounted() {
      const storedTasks = localStorage.getItem('myLocalTask');
      if(storedTasks){
        this.tasks = JSON.parse(storedTasks);
      }
    }
  }

</script>

<style>
  .delete{
    text-decoration: line-through;
    color: red;
  }
</style>

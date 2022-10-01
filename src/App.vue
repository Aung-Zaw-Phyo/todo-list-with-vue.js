<template>

  <div id="app">
    <div class="container py-5">
      <h2 class="text-center">
        {{ title }}
      </h2>
      <div class="mt-5 col-lg-6 mx-auto">
        <div class="input-group">
          <input type="text" v-model="addTask" v-on:keyup.enter="add()" class="form-control" placeholder="Enter your task">
          <button class="btn btn-primary" :onclick="add">Add</button>
        </div>
      </div>
      <div class="row mt-5">
        <div class="col-lg-6 mx-auto">
          <div v-if="filterTasks.length > 0">
            <div class="d-flex justify-content-between mb-4">
              <div class="h4">Action</div>
              <div class="h4">Done</div>
            </div>
            <div class="d-flex justify-content-between mb-2" v-for="(task, index) in filterTasks" :key="index">
              <div :class=" task.done?'text-delete':'' ">{{ task.action }}</div>
              <div> <input type="checkbox" v-model="task.done" /> </div>
            </div>
          </div>
          <div class="alert alert-warning text-center " v-else>
            Your task list is empty!
          </div>
          <div class="mt-3 d-flex justify-content-between">
            <div v-if="tasks.length > 0">
              <button class="btn btn-warning" v-if="hide == false" :onclick="hideShow">Hide Completed Tasks</button>
              <button class="btn btn-primary" v-if="hide == true" :onclick="hideShow">Show Completed Tasks</button>
            </div>
            <div v-if="tasks.length > 0">
              <button class="btn btn-danger" @click="deleteTasks">Delete Completed Tasks</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name : "App",
  data : () => ({
    title: "Todo List",
    addTask: '',
    hide: false,
    tasks: []
  }),
  computed: {
    filterTasks(){
      return this.hide ? this.tasks.filter((task)=> !task.done) : this.tasks;
    }
  },
  methods: {
    hideShow(){
      this.hide ? this.hide = false : this.hide = true;
    },

    add(){
      if(this.addTask == ''){
        return alert('Please add task!');
      }
      this.tasks.push({
        action: this.addTask,
        done: false
      });

      this.storeData();

      this.addTask='';
    },

    deleteTasks() {
      this.tasks = this.tasks.filter( task => !task.done );
      this.storeData();
    },

    storeData() {
      localStorage.setItem('myTasks', JSON.stringify(this.tasks))
    }
  },

  mounted() {
    let datas = localStorage.getItem("myTasks");
    if(datas !== null) {
      this.tasks = JSON.parse(datas);
    }
  }
}
</script>

<style>
.text-delete {
  text-decoration: line-through;
}
</style>

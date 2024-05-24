<template>
  <div class="">
    <h2 class="text-center text-uppercase bg-primary">to do list</h2>

    <div class="container">
      <div class="d-flex">
        <input
          type="text"
          class="form-control"
          v-model="newTask"
          v-on:keyup.enter="save"
        />
        <button class="col-2 btn btn-success" @click="save">save</button>
      </div>
      <div class="" v-if="this.filter.length > 0">
        <div class="row">
          <div class="col-6">product</div>
          <div class="col-6">done</div>
        </div>
        <div
          class="row mt-5"
          v-for="(task, index) in filter"
          v-bind:key="index"
        >
          <div class="col-6" :class="task.done == true ? 'delete' : ''">
            {{ task.product }}
          </div>
          <div class="col-6">
            <input type="checkbox" v-model="task.done" />
          </div>
        </div>
      </div>
      <h3 class="alert alert-warning" v-else>there is no data</h3>
      <div class="row mt-3 bg-warning">
        <div class="col-6">Hide complete</div>
        <div class="col-6">
          <input type="checkbox" v-model="hideComplete" />
        </div>
        <div class="col-12 d-flex justify-content-center">
          <button class="btn btn-danger" @click="Del">delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    hideComplete: false,
    newTask: "",
    tasks: [],
  }),
  computed: {
    filter() {
      return this.hideComplete ? this.tasks.filter((v) => !v.done) : this.tasks;
    },
  },
  methods: {
    save() {
      if (this.newTask === "") {
        alert("please fill the input");
      } else {
        this.tasks.push({
          product: this.newTask,
          done: false,
        });
      }
      this.storeData();
      this.newTask = "";
    },
    Del() {
      this.tasks = this.tasks.filter((v) => !v.done);
      this.storeData();
    },
    storeData() {
      localStorage.setItem("myTask", JSON.stringify(this.tasks));
    },
  },

  created() {
    let data = localStorage.getItem("myTask");
    if (data !== null) {
      this.tasks = JSON.parse(data);
    }
  },
};
</script>

<style>
.delete {
  text-decoration: line-through;
}
</style>

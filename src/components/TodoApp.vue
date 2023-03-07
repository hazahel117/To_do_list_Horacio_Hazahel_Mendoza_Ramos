<template>
  <div class="titulo">
    <h1>Actividad To-do list con Vue</h1>
    <p class="nombre">Horacio Hazahel Mendoza Ramos</p>
  </div>
  <div class="container" style="max-width: 600px">
    
    <h2 class="text-center mt-5 todolist">Pendientes</h2>

    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Ingrese una tarea"
        class="w-100 form-control"
      />
      <button class="btn btn-info" @click="submitTask">AGREGAR</button>
    </div>

    <table class="table table-light table-hover mt-5">
      <thead>
        <tr class="table table-dark">
          <th scope="col">Tarea</th>
          <th scope="col" style="width: 120px">Estado</th>
          <th scope="col" class="text-center">-</th>
          <th scope="col" class="text-center">-</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'hecho' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'pendiente',
                'text-success': task.status === 'hecho',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["pendiente", "hecho"],
      /* Estatdo entre 'pendiente' / 'hecho' */
      tasks: [
        {
          name: "Estudiar para el examen.",
          status: "pendiente",
        },
        {
          name: "Sacar la basura",
          status: "hecho",
        },
      ],
    };
  },
  methods: {
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 1) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    submitTask() {
      if (this.task.length === 0) return;
      
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        
        this.tasks.push({
          name: this.task,
          status: "pendiente",
        });
      }
      this.task = "";
    },
  },
};
</script>
<template>
  <div class="container jumbotron"><!-- agregamos la clase jumbotron para dar el efecto de marco-->
  <h1 class="text center">To Do List</h1><!--encabezado-->


  <!-- START Input aqui escribimos las tarea y las agregamos-->

  <div class="d-flex"> <!--un d-flex para un responsive design en forma de columnas-->
    <input v-model="task" type="text" placeholder="Escribe una tarea" class="form-control"> <!--agregamos la clase form control para un diseño mas amplio-->
    <button @click="submitTask" class="btn btn-warning rounded-0">AGREGAR</button> <!--boton mara agregar la tarea escrita a la lista-->
  </div>

  <!--    ----------------    END Input    ----------------  -->


  <!-- -----------------------task table----------------------->
  <table class="table table-bordered"> <!--clase para dar un estilo con borde-->
  <thead>
    <tr>
      <th scope="col">Tarea</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status === 'finished'}">
          {{task.name}}
          </span>
          </td>
      <td><span @click="changeStatus(index)" class="pointer" :class="{'text-danger':task.status === 'to-do','text-warning':task.status === 'in-progress'}">
        {{task.status}}
        </span>
        </td>
      <td>
        <div class="text-center" @click="editTask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>                       <!-- se utilizo fonts awesome de google por CDN para los iconos de los botones-->
      <td>
        <div class="text-center" @click="deleteTask(index)">
          <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>
  </tbody>
</table>
  <!----------------------- END task table---------------------->
  </div>
</template>



<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () { //creamos una funcion data como objeto
    return {
      task:'',
      editedTask: null,
      availableStatuses: ['to-do','in-progress','finished'],
      tasks: [
        {
          name:'tarea 1',
          status:'to-do'
        },

        {
          name:'tarea 2',
          status:'in progress'
        },
      ]
    }

  },
  methods: {
    submitTask(){
      if(this.task.length === 0 ) return ;

      if(this.editedTask === null){
        this.tasks.push({
        name: this.task,
        status: 'to-do'
        });
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }


      this.task = '';
    },

    deleteTask(index){
      this.tasks.splice (index,1);

    },

    editTask(index){
     this.task = this.tasks[index].name;
     this.editedTask = index;

    },
    changeStatus(index){
    let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
    if(++newIndex > 2) newIndex = 0;
    this.tasks[index].status = this.availableStatuses[newIndex];
    },
  }
};
</script>




<style scoped>
.pointer {
  cursor:pointer;
}

.finished {
  text-decoration: line-through;
}

</style>

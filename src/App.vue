<template>
  <div id="app">
    <navbar-template></navbar-template>
    <div class="container p-3">
      <div class="card p-3">
        <div class="input-group mb-3">
          <input v-model="todo" type="text" class="form-control" placeholder="add todo" aria-label="add todo" aria-describedby="button-addon2">
          <div class="input-group-append">
            <button @click="addTodo()"  class="btn btn-outline-success"  id="button-addon2">Add Todo</button>
          </div>
        </div>
        <!-- <table  class="table table-striped text-center"> -->
        <table v-if="todos.length" class="table table-striped text-center">
          <thead>
            <tr>
              <th width="5%">#</th>
              <th width="45%">Todo</th>
              <th width="25%">Tanggal</th>
              <th width="25%">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(todoo, index) in todos" :key="index" :class="todoo.isChecklist? 'bg-warning' : '' ">
              <th>
                <div class="bg-white shadow">
                  <input :id="`cb-${index}`" v-model="todoo.isChecklist" type="checkbox" >
                  <!-- <input v-model="checklist"> :checked="todoo.isChecklist" type="checkbox" > -->
                </div>
              </th>
              
              <td v-if="!todoo.onEdited && !todoo.isChecklist" :id="index">{{ todoo.text }}</td>
              <td v-else-if="!todoo.onEdited && todoo.isChecklist" :id="index"><del>{{ todoo.text }}</del></td>
              <td v-else>
                <div class="input-group mb-3 w-50 m-auto shadow">
                   <!-- <input :value="todoo.text" type="text" class="form-control" aria-label="Recipients username" aria-describedby="button-addon2"> -->
                   <input v-model="editTodo" type="text" class="form-control" aria-label="Recipients username" aria-describedby="button-addon2">
                   <div class="input-group-append">
                     <button @click="updateTodo(index)" class="btn btn-sm btn-success" type="button" id="button-addon2">Save</button>
                   </div>
                 </div>
                <!-- <input type="text" class="form-control w-25" :value="todoo.text" >
                <button @click="updateTodo(index)" class="btn btn-danger btn-sm mr-2">Update</button> -->
              </td>
              <td>{{ todoo.date }}</td>
              <td>
                <button @click="onUpdate(index)" class="btn btn-secondary btn-sm shadow mr-2">Update</button>
                <button @click="deleteTodo(index)" class="btn btn-danger btn-sm shadow ">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
        <p v-else class="text-center text-danger">Todo masih kosong, buat todo pertama mu!!</p>
      </div>
    </div>
  </div>
</template>

<script>
import navbar from './components/navbar.vue'

export default {
  name: 'App',
  components: {
    'navbar-template': navbar,
  },
  data:
    function () {
      return {
        isUpdate: false,
        editTodo: '',
        todo: '',
        checklist: [],
        todos: [
          {text: 'dumy text', date: '12/12/2002', onEdited: false, isChecklist: false},
        ],
      }
    },

  methods: {
    deleteTodo : function(e) {
      this.todos.splice(e,1)
    },

    addTodo : function() {
      const tgl = new Date()  

      if(this.todo.trim() === ''){
        return alert('todo tidak boleh kosong')
      }

      this.todos.push({
        text: this.todo,
        date: tgl.toLocaleDateString('en-GB'),
        onEdited: false,
        isChecklist: false
      })

      this.todo = ''
    },

    updateTodo : function(i) {
      this.todos[i].text = this.editTodo 
      
      if(this.editTodo.trim() === ''){
        return alert('edit todo tidak boleh kosong')
      }
      
      this.todos[i].onEdited = false 

      this.editTodo = ''
    },

    onUpdate : function(i){
      let isEdit
      this.todos.forEach(val => val.onEdited ? isEdit = true: '' )

      if(isEdit){
        return alert('selesaikan pengeditan terlebih dahulu')
      }

      if(this.todos[i].isChecklist){
        return alert('todo sudah di checklist, unchecklist terlebih dahulu')
      }

      this.editTodo = this.todos[i].text
      this.todos[i].onEdited = true
      
    },
  }
}
</script>

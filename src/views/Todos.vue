<template>
<div>
<h3 class="vue-title">Vue Tables Todos</h3>
<div id="app">
  <v-client-table :data="todos"
          :columns="columns" 
          :options="options">
          <span slot="actions" slot-scope="{row}"> 
            <button @click="edit(todos)">edit</button>
            <button @click="del(todos)">delete</button>
        </span>
  </v-client-table>
</div>
<p class="vue-pagination-ad">
  Like the pagination component and want to use it independently? Try <a target="_blank" href="https://www.npmjs.com/package/vue-pagination-2">vue-pagination-2</a>
</p>
</div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
          form: {
          id: '',
          title: '',
		      completed: ''
        },
        todos: '',
        updateSubmit: false,
            columns: ['id', 'title', 'completed', 'actions'],
            todos: [],
            options: {
                headings: {
                    id: 'ID',
                    title: 'Title',
                    completed:'Completed'.toString()
                },
                sortable: ['title', 'completed'],
                filterable: ['title', 'completed']
            }
        }
    },
    mounted() {
        this.load()
    
    },
    methods: {
    load(){
        axios.get('http://localhost:3000/todos').then(res => {
        this.todos = res.data
      }).catch ((err) => {
        console.log(err);
        
      })
    },
      add(){
      axios.post('http://localhost:3000/todos/', this.form).then(res => {
          this.load()
		  this.form.userId = '1'
          this.form.title = ''
		  this.form.completed = ''
      })
    },
    edit(todos){ 
        this.updateSubmit = true
		this.form.userId = todos.userId
        this.form.id = todos.id 
        this.form.title = todos.title
		this.form.completed = todos.completed
    },
    update(form){ 
       return axios.put('http://localhost:3000/todos/' + form.userId, form.id , {title: this.form.title}, {completed: this.form.completed}).then(res => {
        this.load()
        this.form.id = ''
        this.form.title = ''
		this.form.completed = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
        
      })
    },
    del(todos){
      axios.delete('http://localhost:3000/todos/' + todos.id).then(res =>{
          this.load()
          let index = this.todos.indexOf(form.title)
          this.todos.splice(index,1)
      })
    }
    }
}

</script>

<style>
#app {
  width: 95%;
  margin-top: 50px;
  text-align: center;
}

.VuePagination {
  text-align: center;
}

.vue-title {
  text-align: center;
  margin-bottom: 10px;
}

.vue-pagination-ad {
  text-align: center;
}

.glyphicon.glyphicon-eye-open {
  width: 16px;
  display: block;
  margin: 0 auto;
}

th:nth-child(3) {
  text-align: center;
}

.VueTables__child-row-toggler {
  width: 16px;
  height: 16px;
  line-height: 16px;
  display: block;
  margin: auto;
  text-align: center;
}

.VueTables__child-row-toggler--closed::before {
  content: "+";
}

.VueTables__child-row-toggler--open::before {
  content: "-";
}

</style>
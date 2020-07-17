<template>
  <div id="app">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet">
    <div class="box">
      <!--form input and addtodo button -->
      <div class="form-box">
        <span>
          <input class="input" @keyup.enter="hitEnter()" type="text" placeholder="Add New Todo" v-model="inputTodo">
        </span>
        <span>
          <button class="btn mx-2 btn-primary" @click="addTodo()">ADD TODO</button>
        </span>
      </div>
      <!-- Three buttons to show filtered todos -->
      <div class="my-2">
        <button v-bind:class="{'isActive':isBtn1Active}" class="btn btn-primary ml-2" @click="showAllTodo('btn-1')">
            All Todos
        </button>

        <button v-bind:class="{'isActive':isBtn2Active}" class="btn ml-2 btn-success" @click="showDoneTodo('btn-2')">
              Done Todos
        </button>

        <button v-bind:class="{'isActive':isBtn3Active}" class="btn ml-2 btn-danger" @click="showUndoneTodo('btn-3')">
              Undone Todos
        </button>
        
      </div>
      <!-- All todos when All Todos button is pressed -->
      <div v-if="showAll">
      <div class="todos bg-danger p-2 mt-1 grid" v-for="todo in todos" :key="todo.name" >
        <input type="checkbox" class="mt-3" v-model="todo.done">
        <div class="todo p-2" v-bind:class="{'default':true,'line-through':todo.done}">
          {{todo.name}}
        </div>
        <div class="todo text-white">
          <button class="btn text-danger material-icons" @click="deleteTodo(todo)">
          delete
          </button>
        </div>
      </div>
      </div>
      <!-- All done todos -->
      <div v-if="showDone">
      <div  class="todos bg-danger p-2 mt-1 grid" v-for="todo in doneTodos" :key="todo.name" >
        <input type="checkbox" class="mt-3" v-model="todo.done">
        <div class="todo p-2" v-bind:class="{'default':true,'line-through':todo.done}">
          {{todo.name}}
        </div>
        <div class="todo text-white">
          <button class="btn text-danger material-icons" @click="deleteTodo(todo)">
          delete
          </button>
        </div>
      </div>
      </div>
      <!-- All Undone todos -->
      <div v-if="showUndone">
      <div  class="todos bg-danger p-2 mt-1 grid" v-for="todo in undoneTodos" :key="todo.name" >
        <input type="checkbox" class="mt-3" v-model="todo.done">
        <div class="todo p-2" v-bind:class="{'default':true,'line-through':todo.done}">
          {{todo.name}}
        </div>
        <div class="todo text-white">
          <button class="btn text-danger material-icons" @click="deleteTodo(todo)">
          delete
          </button>
        </div>
      </div>
      </div>
    </div>
    
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  data(){
    return{
      todos:[
        {"id":1,"name":"Do Something","done":false}
      ],
      inputTodo:'',
      id:1,
      showDone:false,
      showUndone:false,
      showAll:true,
      isBtn1Active:false,
      isBtn2Active:false,
      isBtn3Active:false
    }
  },
  computed:{
    
    newTodo:function(){
      return {
        "id":this.id,
        "name":this.inputTodo,
        "done":false
      }
    },
    // filters done todos
    doneTodos:function(){
      return this.todos.filter(x=>x.done===true)
    },
    // filters undone todos
    undoneTodos:function(){
      return this.todos.filter(x=>x.done===false)
    }
  },

  methods:{
    // Adds new todo to todos array
    addTodo:function(){
      if(this.inputTodo!==''){
        this.id+=1
        this.todos.push(this.newTodo)
        this.inputTodo=''
      }
    },
    // Deletes a todo
    deleteTodo:function(todo){
      var index = this.todos.indexOf(todo)
      this.todos.splice(index,1)
    },
    // Adds new todo when enter key is pressed inside input
    hitEnter:function(){
      this.addTodo()
    },
    // Traces the active button
    activeBtn:function(btnId){
      if(btnId==='btn-1'){
        this.isBtn1Active=true
        this.isBtn2Active=false
        this.isBtn3Active=false
      }else if(btnId==='btn-2'){
        this.isBtn1Active=false
        this.isBtn2Active=true
        this.isBtn3Active=false
      }else if(btnId==='btn-3'){
        this.isBtn1Active=false
        this.isBtn2Active=false
        this.isBtn3Active=true
      }
    },

    showDoneTodo:function(btnId){
      this.activeBtn(btnId)
      this.showDone=true
      this.showUndone=false
      this.showAll=false
      
    },
    showUndoneTodo:function(btnId){
      this.showUndone=true
      this.showDone=false
      this.showAll=false
      this.activeBtn(btnId)
    },
    showAllTodo:function(btnId){
      this.activeBtn(btnId)
      this.showUndone=false
      this.showDone=false
      this.showAll=true
      
    },

    
  }
}
</script>

<style>
.default{
  color: antiquewhite;
}
.line-through{
  text-decoration: line-through;
  color: darkgray;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
body{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
   background-image: url('./assets/background-992850_1280.png');
}
#app{
  position: relative;
}
.box{
  position: absolute;
  top: 50px;
  left: 50%;
  transform: translate(-50%);
}
.form-box{
  display: grid;
  grid-template-columns: 3fr 1fr;
}
.input{
  /* width: 300px; */
  width: 100%;
  min-width: 250px;
  padding: .5rem .5rem .4rem .5rem;
  outline: none;
  border:solid 1px blueviolet;
}
.input:focus{
  border: solid 3px blueviolet;
}
.grid{
  display: grid;
  grid-template-columns: 1fr 3fr 1fr;
}

.my-2{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.isActive{
  box-shadow: 0px 0px 5px 5px white;
}
</style>

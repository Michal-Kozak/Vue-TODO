<template>
  <div>
  <div class="app-header">

  <h3 class="app-header-date"> <span style="font-weight:900">Thursday</span>, 10th</h3>
  <p class="task-n">12 Tasks</p>
  <p class="h-month">December</p>
  <hr class="header-hr">
    <div class="header-addtask">
    <button class="header-addtask-btn" v-on:click="seen = !seen"  >
    <svg class="svg-plus" xmlns="http://www.w3.org/2000/svg" width="65" height="65" viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm5 11h-4v4h-2v-4H7v-2h4V7h2v4h4v2z"/>
    </svg>
    </button>
    </div>

 

  
  </div>
    <input type="text"  class="todo-input" v-if="seen" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo"  >   
  <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
  <div class="todo-item-left">
  <input type="checkbox" v-model="todo.completed">
  <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{completed : todo.completed}">{{todo.title}}</div>
  <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
  </div>
  <div class="remove-item" @click="removeTodo(index)">
  &times;
  </div>
  </div>
  </div>
  
</template>

<script>
export default {
  name: 'todo-list',
  data (){
    return{
        seen: false,
        newTodo:'',
        idForTodo:3,
        beforeEditCasche: '',
        todos:[
            {
                'id': 1,
                'title': 'Finish vue Screencast',
                'completed': false,
                'editing': false,
            },
            {
               'id': 2,
                'title': 'smth',
                'completed': false, 
                'editing': false,
            }
        ]
    }
    
  },
  
  directives:{
      focus:{
          inserted: function(el){
              el.focus()
          }
      }
  },
  methods:{
      addTodo(){

           if (this.newTodo.trim().length == 0){
                return
            }

          this.todos.push({
           
              id: this.idForTodo,
              title: this.newTodo,
              completed: false,
            
              
            
          })
          this.newTodo = ''
          this.idForTodo++
          
      },
        editTodo(todo){
            this.beforeEditCasche = todo.title
            todo.editing = true;
        
      },
      
      doneEdit(todo){
           if (todo.title.trim().length == 0){
                todo.title = this.beforeEditCasche
            }
          todo.editing = false
      

      },
      cancelEdit(todo){
          todo.title = this.beforeEditCasche
          todo.editing = false
      

      },
      removeTodo(index){
          this.todos.splice(index,1)
      }
  }
}
</script>


<style lang="scss"  >
.app-header{
    position: relative;
    height:170px;
   
   
}
.app-header-date{
    color:#7073fb;
 position: absolute;
   top: 10px;
  left:30px;
 

}
.task-n{
    font-size:18px;
    font-weight:500;
    font-family:  sans-serif;
    color:gray;
        position: absolute;
   top:35px;
   right: 50px;
   
}

.h-month{
    font-size:22px;
       color:grey;
        position: absolute;
   top: 55px;
   left:35px;
   
}
.header-hr{
  border: 0.7px solid grey;
  position: absolute;
  bottom:20px;
  width:100%;
  opacity:0.3;
}

.header-addtask-btn{
    background-color: white;
    background-repeat:no-repeat;
    border: none;
    cursor:pointer;
    overflow: hidden;
    &:focus{
        outline:0;
    }  
    }    
    .svg-plus{
        position:absolute;
        fill:red;
        bottom:2.5px;
        right:30px;
        background-color:white;
        &:visited{
            fill:green;
        }

    }
.todo-input{
    width:80%;
    padding: 10px 18px;
    font-size: 18px;
    margin-top:10px;
    margin-bottom: 16px;
    border:2px solid #ccc; 
    -webkit-border-radius: 5px;
    border-radius: px;
    
    &:focus{
        outline:0;
    }
    }
    .todo-item{
        margin-bottom:12px;
        display:flex;
        align-items:center;
        justify-content:space-between;
    .remove-item{
        cursor:pointer;
        margin-left:14px;
        &:hover{
            color:black;
        }
    }
    .todo-item-left{
        display:flex;
        align-items:center;
    }
    .todo-item-label{
        padding:10px;
        border: 1px solid whithe;
        margin-left:12px;
    }
    .todo-item-edit{
        font-size:24px;
        color: #2c3e50;
        margin-left:12px;
        width:100%;
        padding:10px;
        border:1px solid #cc;
        
        &:focus{
            outline:none;
        }
    }
    .completed{
        text-decoration: line-through;
        color:grey;
        opacity:0.5;
    }
}
</style>

<template>
  <div>
  <div class="app-header">

  <h3 class="app-header-date"> <span style="font-weight:900">Thursday</span>, 10th</h3>
  <p class="task-n">12 Tasks</p>
  <p class="h-month">December</p>
  <hr class="header-hr">
    <div class="header-addtask">
    
    <button class="header-addtask-btn" v-on:click="seen = !seen"  >
    <svg class="svg-plus" xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24"><path d="M19,11H13V5a1,1,0,0,0-2,0v6H5a1,1,0,0,0,0,2h6v6a1,1,0,0,0,2,0V13h6a1,1,0,0,0,0-2Z"/>
    </svg>
    </button>
    </div>

 

  
  </div>
  <transition name="slide-fade">
    <input type="text"  class="todo-input" v-if="seen" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo"  > </transition>
    <div class="todo-list"> 
 <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
  <div class="todo-item-left">
  <div  >
  <button class="checkbox-btn" v-on:click="iscompleted = !iscompleted" ></button>
 
  <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{iscompleted: iscompleted}" >{{todo.title}}</div>
  <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
  </div>
  </div>
  <div class="remove-item" @click="removeTodo(index)">
  &times;
  </div>
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
        iscompleted: false,
        newTodo:'',
        idForTodo:3,
        beforeEditCasche: '',
        todos:[
            {
                'id': 1,
                'title': 'Finish vue Screencast',
                'iscompleted': false,
                'editing': false,
            },
            {
               'id': 2,
                'title': 'smth',
                'iscompleted': false, 
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
              iscompleted: false,
              
              
            
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


<style lang="scss" scoped >
.app-header{
    position: relative;
    height:170px;
   color:#fbfbff;
   
}
.app-header-date{
    color:#7073fb;
 position: absolute;
   top: 10px;
  left:30px;
 opacity:0.95;

}
.task-n{
    font-size:18px;
    font-weight:500;
    font-family:  sans-serif;
    color:gray;
        position: absolute;
   top:35px;
   right: 50px;
   opacity:0.9;
}

.h-month{
    font-size:22px;
       color:grey;
        position: absolute;
   top: 55px;
   left:35px;
   opacity:0.9;
}
.header-hr{
  border: 0.7px solid grey;
  position: absolute;
  bottom:20px;
  width:100%;
  opacity:0.3;
}

.header-addtask-btn{
    position:absolute;
    bottom:7px;
    right:35px;
    background-color:#fb6a6b;
    border-radius: 50%;
    height: 50px;
  width: 50px;
    border: none;
    cursor:pointer;
    overflow: hidden;
    z-index: 1;
    &:focus{
        outline:0;
    }
    
    
    }    
    .svg-plus{
        fill:white;
    }
.todo-input{
    width:85%;
    padding: 10px 18px;
    font-size: 18px;
    margin-top:10px;
    margin-bottom: 35px;
    border:2px solid #ccc; 
    border-radius:15px;
    -webkit-border-radius: 5px;
    z-index: -1;
    
    
    
    &:focus{
        outline:0;
    }
    }
    .checkbox-btn{
        border: 2px solid black;
        border-radius:5px;
        height:25px;
        width:25px;
        background-color:rgba(128, 128, 128,0.0);
        margin-left:15px;
    }
    .todo-item{
        margin-bottom:8px;
        display:flex;
        align-items:center;
        justify-content:space-between;
         border-bottom: 1.5px solid rgba(128, 128, 128,0.3);
         font-family: 'Titillium Web', sans-serif;
         font-weight:400;
         
         &:first-child{
             margin-top:-20px;
         }
        
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
        position:relative;
        
        
    }
    .todo-item-label{
        padding:10px;
        border: 1px solid white;
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
    .iscompleted{
        text-decoration: line-through;
        
        color:grey;
        opacity:0.5;
    }
}

@import url('https://fonts.googleapis.com/css?family=Titillium+Web:200,300,400,600');
.slide-fade-enter-active {
  transition: all 1s ease ;
}
.slide-fade-leave-active {
 
}
.slide-fade-enter, 
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translatey(-80%);
  opacity:0;
}
.slide-fade-leave-to{
  opacity:0
  

}

</style>

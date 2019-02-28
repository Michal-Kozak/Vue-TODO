<template>
  <div>
  <div class="app-header">

  <h3 class="app-header-date"> <span style="font-weight:900">Thursday</span>, 10th</h3>
  <p class="task-n">{{remaining}} Tasks</p>
  <p class="h-month">December</p>
  <hr class="header-hr">
    <div class="header-addtask">
    
    <button class="header-addtask-btn" @click="seen = !seen"  >
    <svg class="svg-plus" xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24"><path d="M19,11H13V5a1,1,0,0,0-2,0v6H5a1,1,0,0,0,0,2h6v6a1,1,0,0,0,2,0V13h6a1,1,0,0,0,0-2Z"/>
    </svg>
    </button>
    </div>

 

  
  </div>
  
    <input type="text"  class="todo-input" v-if="seen" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo"  >
    <div class="todo-list" > 
 <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
  <div class="todo-item-left">
  <div >
  
  <button class="checkbox-btn" v-on:click="todo.iscompleted = !todo.iscompleted" :class="{svgcheck:todo.iscompleted}" >
    <svg class="checkmark" :class="{svgcheck:todo.iscompleted}" xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24"> <path d="M20.285 2l-11.285 11.567-5.286-5.011-3.714 3.716 9 8.728 15-15.285z"/></svg>   
       </button> 
 
  <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{iscompleted:todo.iscompleted}" >{{todo.title}}</div>
  <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
  </div>
  </div>
  <div class="remove-item" @click="removeTodo(index)">
   
  <svg class="svg-delete" :class="{showdelete:todo.iscompleted}" xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24"><path d="M19,11H5a1,1,0,0,0,0,2H19a1,1,0,0,0,0-2Z"/> </svg>

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
        iscompleted: true,
        newTodo:'',
        idForTodo:3,
        beforeEditCasche: '',
        todos:[
            {
                'id': 1,
                'title': 'Finish vue Screencast',
                'editing': false,
                'iscompleted': false,
            },
            {
               'id': 2,
                'title': 'smth',
                'editing': false,
                'iscompleted': false,
            }
        ]
    }
    
  },
  computed:{
      remaining(){
          return this.todos.filter(todo => !todo.iscompleted).length
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
              editing: false,
              
              
            
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
    z-index:2;
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
    
    .svgcheck{
        opacity:1 !important;
        background-color:#fb6c6d !important;
        
        
    
    }
    .showdelete{
        opacity:1 !important;
    }
    .checkmark{
        opacity:0;
        fill:white;
        transform:scale(1.5);

    }
    .svg-delete{
        opacity:0;
        fill:grey;
        border:2px solid #fb6c6d;
        border-radius:50%;
         &:hover{
            fill:white;
            background-color:#fb6c6d;
        }
        .showdelete{
            
        }
        
    }
  
.todo-input{
    width:85%;
    padding: 10px 18px;
    font-size: 18px;
    margin-top:3px;
    margin-bottom: 45px;
    border:2px solid #ccc; 
    border-radius:15px;
    -webkit-border-radius: 5px;
    z-index: -1;
    
    
    
    &:focus{
        outline:0;
    }
   
    }
    .checkbox-btn{
        display:flex;
        
        border: 1px solid rgba(128,128,128,0.8);
        border-radius:8px;
        height:25px;
        width:25px;
        background-color:rgba(128,128,128,0.12);
        float:left;
        margin-left:-10px;
        margin-right:15px;
        margin-top:7px;
        
    }
    .btn-div{
        outline: 0;
    box-shadow: none!important;
    border:none;
    &:focus{
        outline: 0;
    box-shadow: none!important;
    border:none;
    }
    }
    .todo-item{
       
         display:flex;
        align-items:center;
        justify-content:space-between;
      
         border-bottom: 1.5px solid rgba(128, 128, 128,0.3);
         font-family: 'Titillium Web', sans-serif;
         font-weight:300;
         
         &:first-child{
             margin-top:-20px;
         }
        
    .remove-item{
        cursor:pointer;

        padding:4px;
        margin-top:5px;
        margin-right:10px;
        
    }
    .todo-item-left{
        position:relative;
        left:45px;
        
        
        
    }
    .todo-item-label{
        display:flex;
        
        
        border: 1px solid white;
        
    }
    .todo-item-edit{
        display:flex;
        
        font-size:28px;
        padding-left:10px;
        color: #2c3e50;
        font-family: 'Titillium Web', sans-serif;
        width:90%;
        
        border:2px solid #ccc;
        border-radius:5px;
        
        
        &:focus{
            outline:none;
        }
    }
    .iscompleted{
        text-decoration: line-through;
        text-decoration:uppercase;
        color:grey;
        opacity:0.7;
        
    }
    
}

@import url('https://fonts.googleapis.com/css?family=Titillium+Web:200,300,400,600');


</style>

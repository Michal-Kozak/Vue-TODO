<template>
  <div>
  <div class="app-header">

  <h3 class="app-header-date"> <span style="font-weight:900">Thursday</span>, 10th</h3>
  <p class="task-n">12 Tasks</p>
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
  
  <button class="checkbox-btn" v-on:click="todo.iscompleted = !todo.iscompleted" >
   
  <svg style="opacity:0;"  :class="{svgcheck:todo.iscompleted}" xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24"><path xmlns="http://www.w3.org/2000/svg" d="M18.71,7.21a1,1,0,0,0-1.42,0L9.84,14.67,6.71,11.53A1,1,0,1,0,5.29,13l3.84,3.84a1,1,0,0,0,1.42,0l8.16-8.16A1,1,0,0,0,18.71,7.21Z"/></svg>
       </button> 
 
  <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{iscompleted:todo.iscompleted}" >{{todo.title}}</div>
  <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
  </div>
  </div>
  <div class="remove-item" @click="removeTodo(index)">
  <p class="datetest">8:00   AM</p>
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
                'completed': false,
                'editing': false,
                'iscompleted': false,
            },
            {
               'id': 2,
                'title': 'smth',
                'completed': false, 
                'editing': false,
                'iscompleted': false,
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
        fill:white;
        height:25px;
        width:25px;
        transform:scale(2.8);
        padding-bottom:2px;
    
    
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
        border: 2px solid rgba(128,128,128,0.8);
        border-radius:5px;
        height:25px;
        width:25px;
        background-color:#fb6c6d;
        bottom:0;
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
        font-family: 'Titillium Web', sans-serif;
        font-weight:300;
        font-size:17px;
        padding:6px;
        margin-right:20px;
        &:hover{
            color:black;
        }
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


</style>

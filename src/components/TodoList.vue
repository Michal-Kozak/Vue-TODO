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

    <input type="text" maxlength="35" class="todo-input" v-if="seen" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo"  >
    <div class="todo-list">
        <todo-item v-for="(todo, index) in todos" :key="todo.id" class="todo-item" :todo="todo" :index="index" @removedTodo='removeTodo' @finishedEdit="finishedEdit">
            <TodoItem></TodoItem>
        </todo-item>
    </div>
</div>
</template>

<script>
import TodoItem from './TodoItem'

export default {
    name: 'todo-list',
    components: {
        TodoItem
    },
    data() {
        return {
            seen: false,
            iscompleted: true,
            newTodo: '',
            idForTodo: 3,
            beforeEditCasche: '',
            todos: [{
                    'id': 1,
                    'title': 'Add vue-moment',
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
    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.iscompleted).length
        }
    },

    methods: {
        addTodo() {

            if (this.newTodo.trim().length == 0) {
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
        editTodo(todo) {
            this.beforeEditCasche = todo.title
            todo.editing = true;

        },

        doneEdit(todo) {
            if (todo.title.trim() == '') {
                todo.title = this.beforeEditCasche
            }
            todo.editing = false

        },
        cancelEdit(todo) {
            todo.title = this.beforeEditCasche
            todo.editing = false

        },
        removeTodo(index) {
            this.todos.splice(index, 1)
        },

    }
}
</script>

<style lang="scss" scoped>
.app-header {
    position: relative;
    height: 170px;
    color: #fbfbff;

}

.app-header-date {
    color: #7073fb;
    position: absolute;
    top: 10px;
    left: 30px;
    opacity: 0.95;

}

.task-n {
    font-size: 18px;
    font-weight: 500;
    font-family: sans-serif;
    color: gray;
    position: absolute;
    top: 35px;
    right: 50px;
    opacity: 0.9;
}

.h-month {
    font-size: 22px;
    color: grey;
    position: absolute;
    top: 55px;
    left: 35px;
    opacity: 0.9;
}

.header-hr {
    border: 0.7px solid grey;
    position: absolute;
    bottom: 20px;
    width: 100%;
    opacity: 0.3;
}

.header-addtask-btn {
    position: absolute;
    bottom: 7px;
    right: 35px;
    background-color: #fb6a6b;
    z-index: 2;
    border-radius: 50%;
    height: 50px;
    width: 50px;
    border: none;
    cursor: pointer;
    overflow: hidden;
    z-index: 1;

    &:focus {
        outline: 0;
    }

}

.svg-plus {
    fill: white;
}

.todo-input {
    width: 85%;
    padding: 10px 18px;
    font-size: 18px;
    margin-top: 3px;
    margin-bottom: 45px;
    border: 2px solid #ccc;
    border-radius: 15px;
    -webkit-border-radius: 5px;
    z-index: -1;

    &:focus {
        outline: 0;
    }
}

.btn-div {
    outline: 0;
    box-shadow: none !important;
    border: none;

    &:focus {
        outline: 0;
        box-shadow: none !important;
        border: none;
    }

}

@import url('https://fonts.googleapis.com/css?family=Titillium+Web:200,300,400,600');
</style>

<template>
<div class="todo-item">
    <div class="todo-item-left">
        <div>

            <button class="checkbox-btn" v-on:click="todo.iscompleted = !todo.iscompleted" :class="{svgcheck:todo.iscompleted}" >
    <svg class="checkmark" :class="{svgcheck:todo.iscompleted}" xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24"> <path d="M20.285 2l-11.285 11.567-5.286-5.011-3.714 3.716 9 8.728 15-15.285z"/></svg>   
       </button>

            <div v-if="!editing" @dblclick="editTodo" class="todo-item-label" :class="{iscompleted:todo.iscompleted}">{{todo.title}}

            </div>
            <input  v-else class="todo-item-edit" type="text" maxlength="35" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
  </div>
        </div>
        <div class="remove-item" @click="removeTodo(index)">

            <svg class="svg-delete" :class="{showdelete:todo.iscompleted}" xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24"><path d="M19,11H5a1,1,0,0,0,0,2H19a1,1,0,0,0,0-2Z"/> </svg>

        </div>
    </div>
</template>

<script>
export default {
    name: 'todo-item',
    props: {
        todo: {
            type: Object,
            required: true,
        },
        index: {
            type: Number,
            required: true,
        }
    },
    data() {
        return {
            'id': this.todo.id,
            'title': this.todo.title,
            'iscompleted': this.todo.completed,
            'editing': this.todo.editing,
            'beforeEditCache': '',
        }
    },
    methods: {
        removeTodo(index) {
            this.$emit('removedTodo', index)
        },
        editTodo() {
            this.beforeEditCache = this.title
            this.editing = true;

        },
        doneEdit() {
            if (this.title.trim() == '') {
                this.title = this.beforeEditCache
            }
            this.editing = false

        },
        cancelEdit() {
            this.title = this.beforeEditCache
            this.editing = false
        },
    },
    directives: {
        focus: {
            inserted: function (el) {
                el.focus()
            }
        }
    },
}
</script>

<style lang="scss" scoped>
.svgcheck {
    opacity: 1 !important;
    background-color: #fb6c6d !important;

}

.showdelete {
    opacity: 1 !important;
}

.checkmark {
    opacity: 0;
    fill: white;
    transform: scale(1.5);

}

.svg-delete {
    opacity: 0;
    fill: grey;
    border: 2px solid #fb6c6d;
    border-radius: 50%;

    &:hover {
        fill: white;
        background-color: #fb6c6d;
    }

}

.checkbox-btn {
    display: flex;

    border: 1px solid rgba(128, 128, 128, 0.8);
    border-radius: 8px;
    height: 25px;
    width: 25px;
    background-color: rgba(128, 128, 128, 0.12);
    float: left;
    margin-left: -10px;
    margin-right: 15px;
    margin-top: 7px;

}

.todo-item {

    display: flex;
    align-items: center;
    justify-content: space-between;

    border-bottom: 1.5px solid rgba(128, 128, 128, 0.3);
    font-family: 'Titillium Web', sans-serif;
    font-weight: 300;

    &:first-child {
        margin-top: -20px;
    }

    .remove-item {
        cursor: pointer;

        padding: 4px;
        margin-top: 5px;
        margin-right: 10px;

    }

    .todo-item-left {
        position: relative;
        left: 45px;

    }

    .todo-item-label {
        display: flex;

        border: 1px solid white;

    }

    .todo-item-edit {
        display: flex;
        height: 80%;
        font-size: 23px;
        padding-left: 10px;
        margin-left: 20px;
        color: #2c3e50;
        font-family: 'Titillium Web', sans-serif;
        width: 85%;

        border: 2px solid #ccc;
        border-radius: 5px;

        &:focus {
            outline: none;
        }
    }

    .iscompleted {
        text-decoration: line-through;
        text-decoration: uppercase;
        color: grey;
        opacity: 0.7;

    }

}

@import url('https://fonts.googleapis.com/css?family=Titillium+Web:200,300,400,600');
</style>

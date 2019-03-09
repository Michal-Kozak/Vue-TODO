<template>
  <div id="app" class="app-container">
    <div class="container">
    <todo-list></todo-list>
    </div>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  name: 'app',
  components: {
    TodoList
  }
}
</script>

<style >
body{
  background-image: url("https://scontent-frx5-1.xx.fbcdn.net/v/t1.15752-9/52487992_2245947229056244_5518856142966489088_n.png?_nc_cat=111&_nc_ht=scontent-frx5-1.xx&oh=009a067e167d78c244635e5db434117a&oe=5CF8B65B");
}
*{
  box-sizing: border-box;
}
.app-container{
  
  box-sizing: border-box;
  border: 10px ;
  border-radius: 15px;
  background-color: #fefeff;
  max-width:500px;
  min-width: 400px;
  margin: 0 auto;
}
#app {
  
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 250px;
  font-size:24px;
}
</style>

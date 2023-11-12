<template>
	<div id="app">
    <h1>Todo App</h1>
    <form>
      <label>New Todo</label>
      <input v-model="newTodo">
      <button @click="addTodo()">Add</button>
    </form>
    <h2>Todos</h2>
    <ul>
      <li v-for="(todo,index) in todos" :key="index">
       <p>{{ todo.value }}</p>
        <button @click="remove()">Remove</button>
      </li>
    </ul>
    <h4 v-if="todos.length === 0">Empty list.</h4>
  </div>
</template>


<script setup>
import { ref } from 'vue';
const newTodo = ref('');
const defaultData = [{
	status: false,
	//status是用來判斷動作是否完成
	value: 'Empty list.'
	//事ㄒ預設的資料會讓code更好維護
}]

const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
//將localStorage.getItem("todos")轉成JSON格式
//若localStorage.getItem("todos")為空值，則todosData會等於defaultData


const todos = ref(todosData);
// todosData是todos的預設值,等於todos.value

function addTodo(){
	if(newTodo.value){
		todos.value.push({
			status: false,
			value: newTodo.value
		});
		newTodo.value = '';
	}
	//如果newTodo.value不是空值,就將newTodo.value加入todos.value中並清空newTodo.value
	saveData();
}

function remove(index){
	todos.value.splice(index,1);
	//(index,1)從index開始刪除1個值
	saveData();
}
function saveData(){
	const storageData = JSON.stringify(todos.value);
	//將todos.value轉成JSON格式
	localStorage.setItem("todos",storageData);
	//將storageData這個value存入todos這個檔案中
} 
</script>

<style>
  body {
	margin: 0;
	padding: 0;
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	background-color: #27292d;
	color: white;
}

#app {
	max-width: 600px;
	margin-left: auto;
	margin-right: auto;
	padding: 20px;
}

#app h1 {
	font-weight: bold;
	font-size: 28px;
	text-align: center;
}

#app form {
	display: flex;
	flex-direction: column;
	width: 100%;
}

#app form label {
	font-size: 14px;
	font-weight: bold;
}

#app form input,
#app form button {
	height: 48px;
	box-shadow: none;
	outline: none;
	padding-left: 12px;
	padding-right: 12px;
	border-radius: 6px;
	font-size: 18px;
	margin-top: 6px;
	margin-bottom: 12px;
}

#app form input {
	background-color: transparent;
	border: 2px solid rgba(255, 255, 255, 0.35);
	color: inherit;
}

#app button {
	cursor: pointer;
	background-color: #a0a4d9;
	border: 1px solid #a0a4d9;
	color: #1f2023;
	font-weight: bold;
	outline: none;
	border-radius: 6px;
}

#app h2 {
	font-size: 22px;
	border-bottom: 2px solid rgba(255, 255, 255, 0.35);
	padding-bottom: 6px;
}

#app ul {
	padding: 10px;
}

#app ul li {
	display: flex;
	justify-content: space-between;
	align-items: center;
	border: 2px solid rgba(255, 255, 255, 0.35);
	padding: 12px 24px;
	border-radius: 6px;
	margin-bottom: 12px;
}

#app ul li span {
	cursor: pointer;
}

#app ul li .done {
	text-decoration: line-through;
}

#app ul li button {
	font-size: 12px;
	padding: 6px;
}

#app h4 {
	text-align: center;
	opacity: 0.5;
	margin: 0;
}

</style>

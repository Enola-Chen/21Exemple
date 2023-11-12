<template>
  <h1>ToDo App</h1>
	<form @submit.prevent="addTodo()">
		<label>New ToDo </label>
		<input
			v-model="newTodo"
			name="newTodo"
			autocomplete="off"
		>
		<button>Add ToDo</button>
	</form>
	<h2>ToDo List</h2>
	<ul>
		<li
			v-for="(todo, index) in todos"
			:key="index"
		>
			<span
				:class="{ done: todo.done }"
				@click="doneTodo(todo)"
			>{{ todo.content }}</span>
			<button @click="removeTodo(index)">Remove</button>
		</li>
	</ul>
	<h4 v-if="todos.length === 0">Empty list.</h4>
	<!-- 若todos陣列為空值，則顯示Empty list. -->
</template>


<script setup>
import { ref } from 'vue';
const newTodo = ref('');
			const defaultData = [{
				done: false,
				//done這個名稱可以自己取，但是要跟下面的doneTodo(todo)的todo.done一致
				content: 'Write a blog post'
				//預設的資料會讓code更好維護
				//content這個名稱可以自己取，但是要跟下面的doneTodo(todo)的todo.content一致
		
			}]
			const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
			//JSON.parse() 方法會將一個 JSON 字串，轉換成JS的值或物件。
			//若localStorage.getItem('todos')為空值，則todosData會等於defaultData

			const todos = ref(todosData);
			// todosData是todos的預設值,等於todos.value
			function addTodo () {
				if (newTodo.value) {
					todos.value.push({
						done: false,
						content: newTodo.value
					});
					newTodo.value = '';
				}
				//觸發addTodo事件後將input的值加入todos.value中
				//將值存入todos.value後，將input的值清空
				saveData();
			}

			function doneTodo (todo) {
				todo.done = !todo.done
				saveData();
				//這是一個切換的功能，若todo.done為true，則todo.done會變成false
				//為何要切換?因為我們要讓使用者知道哪些事項已完成，哪些事項還沒完成
			}

			function removeTodo (index) {
				todos.value.splice(index, 1);
				//splice(start, deleteCount, item1, item2, ...) 
				saveData();
			}

			function saveData () {
				//每個行為執行後都要將資料存入localStorage
				const storageData = JSON.stringify(todos.value);
				//JSON.stringify() 方法會將一個 JavaScript 值(物件或陣列)轉換成一個 JSON 字串,並將資料存入localStorage
				localStorage.setItem('todos', storageData);
				//localStorage.setItem(fieldname, fieldvalue)
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

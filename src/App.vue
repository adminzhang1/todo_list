<template>
	<div id="root">
		<div class="todo-container">
			<div class="todo-wrap">
				<HeaderVue @addTodo="addTodo"></HeaderVue>
				<ListVue :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"></ListVue>
				<FooterVue :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"></FooterVue>
			</div>
		</div>
	</div>
</template>

<script>
	import pubsub from 'pubsub-js'
	import HeaderVue from './components/Header';
	import ListVue from './components/List';
	import FooterVue from './components/Footer';

	export default {
		name: 'App',
		components: {
			HeaderVue,ListVue,FooterVue
		},
		data(){
			return {
				todos: JSON.parse(localStorage.getItem('todos')) || []
			}
		},
		methods: {
			// 添加信息
			addTodo(todoObj){
				this.todos.unshift(todoObj)
			},
			// 勾选or取消勾选
			checkTodo(id){
				this.todos.forEach(item => {
					if(item.id === id) item.done = !item.done
				})
			},
			// 删除一个数据
			deleteTodo(_,id){
				this.todos = this.todos.filter(item => item.id !== id)
			},
			// 全选or取消全选
			checkAllTodo(done){
				this.todos.forEach(item => {
					item.done = done
				})
			},
			// 清除所有已完成的
			clearAllTodo(){
				this.todos = this.todos.filter(item => !item.done)
			},
			// 更新一个数据
			updataTodo(id,title){
				this.todos.forEach(item => {
					if(item.id === id) item.title = title
				})
			},
		},
		watch: {
			todos:{
				deep: true,
				handler(value){
					localStorage.setItem('todos',JSON.stringify(value))
				}
			}
		},
		mounted(){
			this.$bus.$on('checkTodo',this.checkTodo)
			this.$bus.$on('updataTodo',this.updataTodo)
			this.pubId = pubsub.subscribe('deleteTodo',this.deleteTodo)
		},
		beforeDestroy(){
			this.$bus.$off(['checkTodo','updataTodo'])
			pubsub.unsubscribe(this.pubId)
		}
	}
</script>

<style>
	/*base*/
	body {
		background: #fff;
	}
	.btn {
		display: inline-block;
		padding: 4px 12px;
		margin-bottom: 0;
		font-size: 14px;
		line-height: 20px;
		text-align: center;
		vertical-align: middle;
		cursor: pointer;
		box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
		border-radius: 4px;
	}
	.btn-danger {
		color: #fff;
		background-color: #da4f49;
		border: 1px solid #bd362f;
	}
	.btn-edit{
		color: #fff;
		background-color: skyblue;
		border: 1px solid rgb(103, 159, 180);
		margin-right: 5px;
	}
	.btn-danger:hover {
		color: #fff;
		background-color: #bd362f;
	}
	.btn:focus {
		outline: none;
	}
	.todo-container {
		width: 600px;
		margin: 0 auto;
	}
	.todo-container .todo-wrap {
		padding: 10px;
		border: 1px solid #ddd;
		border-radius: 5px;
	}
</style>
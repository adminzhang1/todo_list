<template>
	<div class="todo-header">
		<input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add" v-model="title"/>
	</div>
</template>

<script>
	import { v4 as uuidv4 } from 'uuid'
	export default {
		name: 'Header',
		// props: ['addTodo'],
		data(){
			return {
				title: ''
			}
		},
		methods: {
			add(e){
				// 校验数据
				if(!this.title.trim()) return alert('填写值不能为空')
				// 将用户的输入包装成一个todo对象
				const todoObj = { id: uuidv4(), title: e.target.value, done: false }
				// 将新输入的数据添加到app中
				// this.addTodo(todoObj)
				this.$emit('addTodo',todoObj)
				// 清空输入
				this.title = ''
			}
		}
	}
</script>

<style scoped>
	/*header*/
	.todo-header input {
		width: 560px;
		height: 28px;
		font-size: 14px;
		border: 1px solid #ccc;
		border-radius: 4px;
		padding: 4px 7px;
	}
	.todo-header input:focus {
		outline: none;
		border-color: rgba(82, 168, 236, 0.8);
		box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
	}
</style>
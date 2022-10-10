<template>
	<li>
		<label>
			<input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)"/>
			<span v-show="!todo.isEdit">{{ todo.title }}</span>
			<input
				v-show="todo.isEdit"
				type="text"
				:value="todo.title"
				@blur="handleBlur(todo,$event)"
				ref="inputTitle"
			>
		</label>
		<button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
		<button class="btn btn-edit" @click="handleEdit(todo)" v-show="!todo.isEdit">编辑</button>
	</li>
</template>

<script>
	import pubsub from 'pubsub-js';
	export default {
		name: 'Item',
		// 声明接收todoObj对象
		props: ['todo'],
		methods: {
			// 勾选or取消勾选
			handleCheck(id){
				this.$bus.$emit('checkTodo',id)
			},
			// 删除
			handleDelete(id){
				if(confirm('确定删除吗?')){
					// this.$bus.$emit('deleteTodo',id)
					pubsub.publish('deleteTodo',id)
				}
			},
			// 编辑
			handleEdit(todo){
				if(todo.hasOwnProperty('isEdit')){
					todo.isEdit = true
				}else{
					this.$set(todo,'isEdit',true)
				}
				this.$nextTick(() => {
					this.$refs.inputTitle.focus()
				})
			},
			// 修改信息事件
			handleBlur(todo,e){
				todo.isEdit = false
				if(!e.target.value.trim()) return alert('输入不能为空!')
				this.$bus.$emit('updataTodo',todo.id,e.target.value)
			}
		}
	}
</script>

<style scoped>
	/*item*/
	li {
		list-style: none;
		height: 36px;
		line-height: 36px;
		padding: 0 5px;
		border-bottom: 1px solid #ddd;
	}
	li label {
		float: left;
		cursor: pointer;
	}
	li label li input {
		vertical-align: middle;
		margin-right: 6px;
		position: relative;
		top: -1px;
	}
	li button {
		float: right;
		display: none;
		margin-top: 3px;
	}
	li:before {
		content: initial;
	}
	li:last-child {
		border-bottom: none;
	}
	li:hover{
		background-color: pink;
	}
	li:hover button{
		display: block;
	}
</style>
<template>
	<div class="todo-footer" v-if="total">
		<label>
			<input type="checkbox" :checked="isAll" @change="checkAll"/>
		</label>
		<span>
			<span>已完成{{ doneTotal }}</span>/全部{{ total }}
		</span>
		<button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
	</div>
</template>

<script>
	export default {
		name: 'Footer',
		props: ['todos','checkAllTodo','clearAllTodo'],
		computed: {
			total(){
				return this.todos.length
			},
			doneTotal(){
				// return this.todos.filter(item => item.done).length
				return this.todos.reduce((pre,current) => {
					if(current.done) return pre += 1
					return pre
				},0)
			},
			isAll(){
				return this.doneTotal === this.total && this.total > 0
			}
		},
		methods: {
			checkAll(e){
				this.checkAllTodo(e.target.checked)
			},
			clearAll(){
				this.clearAllTodo()
			}
		}
	}
</script>

<style scoped>
	/*footer*/
	.todo-footer {
		height: 40px;
		line-height: 40px;
		padding-left: 6px;
		margin-top: 5px;
	}
	.todo-footer label {
		display: inline-block;
		margin-right: 20px;
		cursor: pointer;
	}
	.todo-footer label input {
		position: relative;
		top: -1px;
		vertical-align: middle;
		margin-right: 5px;
	}
	.todo-footer button {
		float: right;
		margin-top: 5px;
	}
</style>
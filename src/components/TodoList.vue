<template lang="pug">
	
	div(v-if="todos.length" id="distanceLogsContainer" class="app-logs__wrap")
		TodoListItem(
			v-for="todo in todos"
			:key="todo.id"
			:todo="todo"
		)
	p(v-else) Журнал запросов пуст.

</template>

<script>

import TodoListItem from './TodoListItem.vue'

export default {
	components: {
		TodoListItem
	},
	props: {
		todos: Array
	}
}
</script>

<style lang="less">
	.app-logs__wrap {
	    margin: 30px 0 0 0;
	}
</style>
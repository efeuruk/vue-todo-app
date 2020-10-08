<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class TodoList extends Vue {
	idForTodo: number = 3;
	newTodo: string = '';
	todos: Object[] = [
		{
			id: '1',
			title: 'Finish Vue.JS',
			isDone: false,
		},
		{
			id: '2',
			title: 'Quit Video Games',
			isDone: false,
		},
	];

	mounted() {
		this.newTodo = '';
	}

	updateNewTodo(e: any) {
		this.newTodo = e.target.value;
	}

	addTodo() {
		if (this.newTodo.length === 0) return;
		this.todos.push({
			id: this.idForTodo,
			title: this.newTodo,
			isDone: false,
		});

		this.newTodo = '';
		this.idForTodo++;
	}

	removeTodo(index: number) {
		this.todos.splice(index, 1);
	}
}
</script>

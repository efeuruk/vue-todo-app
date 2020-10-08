<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

type Todo = {
	id: number;
	title: string;
	isDone: boolean;
	editing: boolean;
};

@Component({
	directives: {
		focus: {
			inserted: function(el) {
				el.focus();
			},
		},
	},
})
export default class TodoList extends Vue {
	idForTodo: number = 3;
	newTodo: string = '';
	beforeEditCache: string = '';
	todos: Todo[] = [
		{
			id: 1,
			title: 'Finish Vue.JS',
			isDone: false,
			editing: false,
		},
		{
			id: 2,
			title: 'Quit Video Games',
			isDone: false,
			editing: false,
		},
	];

	// mounted() {}

	addTodo() {
		if (this.newTodo.length === 0) return;

		this.todos.push({
			id: this.idForTodo,
			title: this.newTodo,
			isDone: false,
			editing: false,
		});

		this.newTodo = '';
		this.idForTodo++;
	}

	removeTodo(index: number) {
		this.todos.splice(index, 1);
	}

	editTodo(todo: Todo, isEdit: boolean = true) {
		this.beforeEditCache = todo.title;
		todo.editing = isEdit;
		if (!isEdit && todo.title.length !== 0) {
			todo.title = this.beforeEditCache;
		}
	}

	cancelEdit(todo: Todo) {
		todo.title = this.beforeEditCache;
		todo.editing = false;
	}
}
</script>

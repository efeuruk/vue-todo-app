<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import gql from 'graphql-tag';

type Todo = {
	description: string;
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
	apollo: {
		todos: gql`
			query {
				todos(order_by: { description: asc }) {
					description
					id
					isDone
				}
			}
		`,
		$loadingKey: 'loading',
	},
})
export default class TodoList extends Vue {
	newTodo: string = '';
	beforeEditCache: string = '';
	todos: Todo[] = [];

	mounted() {
		console.log(this.$apollo);
	}

	async addTodo() {
		if (this.newTodo.length === 0) return;

		await this.$apollo.mutate({
			mutation: gql`
				mutation($description: String!) {
					insert_todos_one(object: { description: $description }) {
						description
					}
				}
			`,
			variables: {
				description: this.newTodo,
			},
		});

		this.todos.push({
			description: this.newTodo,
			isDone: false,
			editing: false,
		});

		this.newTodo = '';
	}

	removeTodo(index: number) {
		this.todos.splice(index, 1);
	}

	editTodo(todo: Todo, isEdit: boolean = true) {
		this.beforeEditCache = todo.description;
		todo.editing = isEdit;
		if (!isEdit && todo.description.length !== 0) {
			todo.description = this.beforeEditCache;
		}
	}

	cancelEdit(todo: Todo) {
		todo.description = this.beforeEditCache;
		todo.editing = false;
	}
}
</script>

<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import getTodos from '../../graphql/getTodos.gql';
import addTodo from '../../graphql/addTodo.gql';
import deleteTodo from '../../graphql/deleteTodo.gql';
import updateTodo from '../../graphql/updateTodo.gql';
import toggleIsDone from '../../graphql/toggleIsDone.gql';

type Todo = {
	id: number;
	description: string;
	isDone: boolean;
};

@Component({
	directives: {
		focus: {
			inserted(el) {
				el.focus();
			},
		},
	},
	apollo: {
		todos: getTodos,
	},
})
export default class TodoList extends Vue {
	public newTodo: string = '';
	public beforeEditCache: string = '';

	public mounted() {
		// console.log(this.$apollo);
	}

	public async addTodo() {
		if (this.newTodo.length === 0) {
			return;
		}

		await this.$apollo.mutate({
			mutation: addTodo,
			variables: {
				description: this.newTodo,
			},
		});

		this.$apollo.queries.todos.refetch();
		this.newTodo = '';
	}

	public async removeTodo(id: number) {
		await this.$apollo.mutate({
			mutation: deleteTodo,
			variables: {
				id,
			},
		});

		this.$apollo.queries.todos.refetch();
	}

	// public editTodo(todo: Todo, isEdit: boolean = true) {
	// 	console.log(todo);
	// 	// this.beforeEditCache = todo.description;
	// 	// todo.editing = isEdit;
	// 	// if (!isEdit && todo.description.length !== 0) {
	// 	// 	todo.description = this.beforeEditCache;
	// 	// }
	// }

	// public cancelEdit(todo: Todo) {
	// 	todo.description = this.beforeEditCache;
	// 	todo.editing = false;
	// }

	public async toggleIsDone(todo: Todo) {
		await this.$apollo.mutate({
			mutation: toggleIsDone,
			variables: {
				id: todo.id,
				isDone: !todo.isDone,
			},
		});

		this.$apollo.queries.todos.refetch();
	}
}
</script>

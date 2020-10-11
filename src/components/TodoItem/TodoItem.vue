<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import getTodos from '../../graphql/getTodos.gql';
import deleteTodo from '../../graphql/deleteTodo.gql';
import updateTodo from '../../graphql/updateTodo.gql';
import toggleIsDone from '../../graphql/toggleIsDone.gql';

type Todo = {
	id: number;
	description: string;
	isDone: boolean;
} | null;

@Component({
	apollo: {
		todos: getTodos,
	},
	directives: {
		focus: {
			inserted(el) {
				el.focus();
			},
		},
	},
})
export default class TodoList extends Vue {
	public beforeEditCache: string = '';
	public editedTodo: Todo = null;
	@Prop() public todo!: Todo;

	public async removeTodo(id: number) {
		await this.$apollo.mutate({
			mutation: deleteTodo,
			variables: {
				id,
			},
		});

		this.$apollo.queries.todos.refetch();
	}

	public editTodo(todo: Todo) {
		todo && (this.beforeEditCache = todo.description);
		this.editedTodo = todo;
	}

	public async doneEdit(todo: Todo) {
		if (todo?.description.length !== 0 && todo !== null) {
			this.editedTodo = null;
			await this.$apollo.mutate({
				mutation: updateTodo,
				variables: {
					id: todo.id,
					description: todo.description,
				},
			});
		}
	}

	public cancelEdit(todo: Todo) {
		this.editedTodo = null;
		todo && (todo.description = this.beforeEditCache);
	}

	public async toggleIsDone(todo: Todo) {
		todo &&
			(await this.$apollo.mutate({
				mutation: toggleIsDone,
				variables: {
					id: todo.id,
					isDone: !todo.isDone,
				},
			}));
	}
}
</script>

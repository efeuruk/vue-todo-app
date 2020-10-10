<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import getTodos from '../../graphql/getTodos.gql';
import addTodo from '../../graphql/addTodo.gql';
import deleteTodo from '../../graphql/deleteTodo.gql';
import updateTodo from '../../graphql/updateTodo.gql';
import toggleIsDone from '../../graphql/toggleIsDone.gql';

type Todo = {
	id: number;
	description: string;
	isDone: boolean;
} | null;

const filters: any = {
	all: (todos: Todo[]) => todos,
	active: (todos: Todo[]) =>
		todos.filter((todo: Todo) => todo && !todo.isDone),
	completed: (todos: Todo[]) =>
		todos.filter((todo: Todo) => todo && todo.isDone),
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
	watch: {},
})
export default class TodoList extends Vue {
	public newTodo: string = '';
	public beforeEditCache: string = '';
	public editedTodo: Todo = null;
	public visibility: string = 'all';

	public filteredTodos() {
		return filters[this.visibility](this.todos);
	}

	public onHashChange() {
		const visibility = window.location.hash.replace(/#\/?/, '');
		if (filters[visibility]) {
			this.visibility = visibility;
		} else {
			window.location.hash = '';
			this.visibility = 'all';
		}
	}

	mounted() {
		this.onHashChange();
		window.addEventListener('hashchange', this.onHashChange);
	}

	public remainingTodos = () => filters.active(this.todos).length;

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

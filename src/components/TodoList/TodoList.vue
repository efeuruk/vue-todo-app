<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import TodoItem from '../TodoItem/TodoItem.vue';
import FilterButton from '../FilterButton/FilterButton.vue';
import getTodos from '../../graphql/getTodos.gql';
import addTodo from '../../graphql/addTodo.gql';

type Todo = {
	id: number;
	description: string;
	isDone: boolean;
} | null;

const filters: any = {
	all: (todos: Todo[]) => todos,
	active: (todos: Todo[]) =>
		todos.filter((todo: Todo) => todo && !todo.isDone),
	done: (todos: Todo[]) => todos.filter((todo: Todo) => todo && todo.isDone),
};

@Component({
	apollo: {
		todos: getTodos,
	},
	components: {
		TodoItem,
		FilterButton,
	},
})
export default class TodoList extends Vue {
	public newTodo: string = '';
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

	public mounted() {
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
}
</script>

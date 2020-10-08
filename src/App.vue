<template>
	<div id="app" class="container">
		<TodoList />
	</div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import TodoList from './components/TodoList/TodoList.vue';

@Component({
	components: {
		TodoList,
	},
})
export default class App extends Vue {}
</script>

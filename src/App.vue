<template>
	<div id="app" class="container">
		<!-- <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" /> -->
		<TodoList />
	</div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import HelloWorld from './components/HelloWorld.vue';
import TodoList from './components/TodoList/TodoList.vue';

@Component({
	components: {
		HelloWorld,
		TodoList,
	},
})
export default class App extends Vue {}
</script>

<style lang="scss">
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
	font-size: 22px;
}

.container {
	max-width: 600px;
	margin: 0 auto;
}
</style>

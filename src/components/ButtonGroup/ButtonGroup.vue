<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
import FilterButton from '../FilterButton/FilterButton.vue';

type Todo = {
	id: number;
	description: string;
	isDone: boolean;
} | null;

@Component({
	components: {
		FilterButton,
	},
})
export default class ButtonGroup extends Vue {
	@Prop() public todos!: Todo[];
	@Prop() public visibility!: string;
	public labels: string[] = ['all', 'active', 'done'];
}
</script>

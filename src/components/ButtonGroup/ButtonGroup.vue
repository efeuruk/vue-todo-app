<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
import FilterButton from '../FilterButton/FilterButton.vue';

@Component({
	components: {
		FilterButton,
	},
})
export default class ButtonGroup extends Vue {
	@Prop() private todos!: any;
	@Prop() private visibility!: string;
	private labels: string[] = ['all', 'active', 'done'];
}
</script>

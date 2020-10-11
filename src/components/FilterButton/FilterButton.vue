<style lang="scss" src="./_style.scss"></style>
<template src="./template.html"></template>

<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';

@Component({})
export default class FilterButton extends Vue {
	@Prop() private label!: string;
	@Prop() private visibility!: string;
}
</script>

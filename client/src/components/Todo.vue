<template>
	<v-card
		class="ma-4 back"
		max-width="400"
		height="400"
		:elevation="2"
	>
	<v-toolbar
		class="toolbar"
		height="50"
	>
		Todo List
	</v-toolbar>
		<p>Herro</p>
	</v-card>
</template>

<script>
export default {
	name: 'Todo',
};
</script>

<style>
.toolbar {
	position: sticky;
	top: 0;
	z-index: 999;
}
</style>

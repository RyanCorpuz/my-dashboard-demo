<template>
	<v-app-bar
		app
		permanent
		clipped-left
		flat
		dense
		:elevation="2"
	>
		<v-toolbar-title>
			Let's Get It Today
		</v-toolbar-title>
		<v-spacer></v-spacer>
		<v-btn
			class="mx-3"
			v-for="item in items"
			v-on:click="go(item.ref)"
			v-bind:key="item.name"
			x-small
		>
			<v-icon small>{{item.icon}}</v-icon>
		</v-btn>
	</v-app-bar>
</template>

<script>
export default {
	name: 'AppBar',
	
	data() {
		return {
			items: [
				{ 
					name: "gmail",
					ref: "https://mail.google.com/",
					icon: "mdi-email"
				},
				{
					name: "drive",
					ref: "https://drive.google.com/",
					icon: "mdi-google-drive"
				},
				{
					name: "messages",
					ref: "https://messages.google.com/web",
					icon: "mdi-message-text"
				},
				{
					name: "github",
					ref: "https://github.com/",
					icon: "mdi-github"
				},
				{
					name: "youtube",
					ref: "https://youtube.com",
					icon: "mdi-youtube"
				},
				{
					name: "spotify",
					ref: "https://open.spotify.com/",
					icon: "mdi-spotify"
				},
			]
		}
	},
	methods: {
		go(ref) {
			window.open(ref);
		},
	}
};
</script>

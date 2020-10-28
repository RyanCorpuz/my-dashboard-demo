<template>
	<v-app>
		<AppBar />
		<NavDrawer />
		<v-main>
			<v-row>
				<v-col cols="12" sm="6" md="3">
					<Clock />
				</v-col>
				<v-col cols="12" sm="8" md="5">
					<EventTracker />
				</v-col>
				<v-col cols="12" md="12">
					<JobHuntTracker />
				</v-col>
			</v-row>
		</v-main>
	</v-app>
</template>

<script>

import AppBar from './components/AppBar';
import NavDrawer from './components/NavDrawer';
import Clock from './components/Clock';
import JobHuntTracker from './components/JobHuntTracker';
import EventTracker from './components/EventTracker';


export default {
	name: 'App',

	components: {
		AppBar,
		NavDrawer,
		Clock,
		JobHuntTracker,
		EventTracker,
	},
};
</script>

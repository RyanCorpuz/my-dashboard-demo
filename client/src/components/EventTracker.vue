<template>
	<v-card
		class="ma-4"
		width="95%"
		height="400"
		style="overflow-y:auto"
	>
		<v-toolbar
			height="50"
			class="toolbar"
		>
			<v-row>
				<v-col cols="12" sm="6">
					<v-card-title>Event Tracker</v-card-title>
				</v-col>
			</v-row>
			<EventTrackerForm v-on:add-event="addEvent" />
			<v-dialog
				v-model="dialog"
				max-width="400px"
				class="ma-2 "
				persistent
				>
				<v-card>
					<v-card-title class="justify-center">
						Record Log
					</v-card-title>
					<v-date-picker
						v-model="date"
						no-title
						scrollable
						:max="getTodayDate"
						width="90%"
					>
						<v-btn
						text
						color="primary"
						@click="close"
						>
							Cancel
						</v-btn>
						<v-spacer></v-spacer>
						<v-btn
						text
						color="primary"
						@click="addLog"
						:disabled="!date"
						>
							OK
						</v-btn>
					</v-date-picker>
				</v-card>
			</v-dialog>
		</v-toolbar>
		<v-data-table
			:search="search"
			:headers="headers"
			:items="events"
			sort-by="dates[0]"
			sort-desc
			disable-pagination
			hide-default-footer
			dense
		>
			<template v-slot:top>
				<v-text-field
					v-model="search"
					label="Search"
					class="mx-4"
				></v-text-field>
			</template>
			<template v-slot:item.days="{ item }">
				{{daysSince(item.dates)}}
			</template>
			<template v-slot:item.date="{ item }">
				{{displayDate(item.dates)}}
			</template>
			<template v-slot:item.log="{ item }">
				<v-btn
					x-small
					icon
					@click="select(item)"
					>
					<v-icon
						small
					>
						mdi-plus
					</v-icon>
				</v-btn>
			</template>
		</v-data-table>
	</v-card>
</template>

<script>
import EventTrackerForm from './EventTrackerForm'


const week = ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'];
const month = ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'];

export default {
	name: 'EventTracker',
	components: {
		EventTrackerForm,
	},
	data() {
		return {
			defaultDate: '',
			search: '',
			dialog: false,
			headers: [
				{ text: 'Log', value: 'log', align: 'center', sortable: false },
				{ text: 'Event', value: 'description', align: 'left' },
				{ text: 'Days Since', value: 'days', align: 'left' },
				{ text: 'Last Time', value: 'date', align: 'left' },
			],
			events: [
				{
					description: 'Room Purge',
					dates: [1592620150000],
				},
				{
					description: 'Ate Fast Food',
					dates: [1602670150000],
				},
				{
					description: 'Oil Change',
					dates: [1600620150000],
				},
				{
					description: 'Change Headlights',
					dates: [1578620150000],
				},
				{
					description: 'Cleaned Laptop',
					dates: [1603670150000],
				},
			],
			selectedItem: null,
			date: null,
		}
	},
	computed: {
		getTodayDate() {
			const now = new Date()
			return now.getFullYear() + '-' + (now.getMonth() + 1) + '-' + (now.getDate())
		},
	},
	mounted() {
		const now = new Date();
		this.date = now.getFullYear() + '-' + (now.getMonth() + 1) + '-' + (now.getDate())
	},
	methods: {
		addEvent(newEvent){ 
			this.events.push(newEvent);
		},
		addLog() {
			const now = new Date()
			const offset = now.getTimezoneOffset() * 60 * 1000;
			const thedate = Date.parse(new Date(this.date)) + offset;
			var index = this.events.findIndex(x => {
				return x.description === this.selectedItem.description; });
			this.events[index].dates.push(thedate);
			this.events[index].dates = this.events[index].dates.sort( (a,b) => {
				return b - a;
			} )
			this.close();
		},
		select(item) {
			this.dialog = true;
			this.selectedItem = item;
		},
		close() {
			this.dialog = false;
			const now = new Date();
			this.date = now.getFullYear() + '-' + (now.getMonth() + 1) + '-' + (now.getDate())
			this.selectedItem = null;
		},
		convertMsToDate(date) {
			return Number(date);
		},
		displayDate(dates) {
			if(dates.length){
				const day = new Date(this.convertMsToDate(dates[0]));
				return week[day.getDay()] + ' ' + month[day.getMonth()] + ' ' + day.getDate() + ', ' + day.getFullYear()
			}
		},
		daysSince(dates) {
			if(dates.length){
				const lasttime = Date.parse(new Date(this.convertMsToDate(dates[0])));
				const today = Date.parse(new Date());
				const daysago = Math.floor((today - lasttime) / (1000 * 60 * 60 * 24))
				if (daysago === 1) { return "Yesterday"; }
				else if (daysago === 0) { return "Today"; }
				else { return daysago + " days ago"; }
			}
			else {
				return 'No Record';
			}

		},
	},
};
</script>

<style>
.toolbar {
	position: sticky;
	top: 0;
}
.v-input__append-inner {
	margin: 0;
}
</style>

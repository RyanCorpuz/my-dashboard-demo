<template>
	<v-card
		class="ma-4"
		width="95%"
		height="400"
		style="overflow:auto"
	>
		<v-toolbar
			height="50"
			class="toolbar"
		>
				<v-toolbar-title>Job Hunt Tracker</v-toolbar-title>
				<v-spacer></v-spacer>
				<v-col cols="3"
					class="d-none d-sm-flex"
				>
					<v-card-text>
						Total: {{totalApplied}}
					</v-card-text>
				</v-col>
				<v-col cols="5"
					class="d-none d-sm-flex"
				>
					<v-card-text>
						Last 7 Days: {{lastSevenDaysApplied}}
					</v-card-text>
				</v-col>
			<JobHuntTrackerForm v-on:add-job="addJob" />
		</v-toolbar>
		<v-data-table
			:search="search"
			:headers="headers"
			:items="jobs"
			sort-by="date"
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
			<template v-slot:item.date="{item}">
				{{display(item.date)}}
			</template>
			<template v-slot:item.status="{item}">
				<v-select
					v-model="item.status"
					:items="state"
					height="15"
					class="ma-0"
				>
				{{item.status}}
				</v-select>
			</template>
		</v-data-table>
	</v-card>
</template>

<script>
import JobHuntTrackerForm from './JobHuntTrackerForm'


const week = ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'];
const month = ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'];

export default {
	name: 'JobHuntTracker',
	components: {
		JobHuntTrackerForm,
	},
	data() {
		return {
			search: '',
			state: ['Applied', 'Interview', 'Rejection', 'Offered'],
			dialog: false,
			headers: [
				{ text: 'Job', value: 'job', align: 'start' },
				{ text: 'Company', value: 'company', align: 'left' },
				{ text: 'City', value: 'city', align: 'left' },
				{ text: 'Province', value: 'prov', align: 'left' },
				{ text: 'Date Applied', value: 'date', align: 'left' },
				{ text: 'Status', value: 'status', align: 'left', width: '135px' },
			],
			jobs: [
				{
					job: 'Operations Manager',
					company: 'Adecco Canada',
					city: 'Edmonton',
					prov: 'AB',
					date: 1592620150000,
					status: 'Applied',
				},
				{
					job: 'Graphic Designer',
					company: 'Mathison Academy Inc',
					city: 'Vancouver',
					prov: 'BC',
					date: 1592620150000,
					status: 'Applied',
				},
				{
					job: 'Car Detailer',
					company: 'Enterprise Holdings',
					city: 'Calgary',
					prov: 'AB',
					date: 1582620150000,
					status: 'Applied',
				},
				{
					job: 'Cook',
					company: 'Sushi Uomo',
					city: 'Burnaby',
					prov: 'BC',
					date: 1578620150000,
					status: 'Applied',
				},
				{
					job: 'Software Engineer, Product',
					company: 'Brex',
					city: 'Vancouver',
					prov: 'BC',
					date: 1600920150000,
					status: 'Applied',
				},
				{
					job: 'Branch Administrator',
					company: 'RBC',
					city: 'Toronto',
					prov: 'ON',
					date: 1600620150000,
					status: 'Applied',
				},
				{
					job: 'Security Guard',
					company: 'G4S',
					city: 'Calgary',
					prov: 'AB',
					date: 1601130150000,
					status: 'Applied',
				},
				{
					job: 'Barista',
					company: 'Starbucks',
					city: 'Richmond',
					prov: 'BC',
					date: 1602410150000,
					status: 'Interview',
				},
				{
					job: 'Marketing Team Lead',
					company: 'Stantec',
					city: 'Toronto',
					prov: 'ON',
					date: 1579500150000,
					status: 'Rejection',
				},
				{
					job: 'Manager',
					company: 'McDonald\'s',
					city: 'Red Deer',
					prov: 'AB',
					date: 1602670150000,
					status: 'Applied',
				},
				{
					job: 'Personal Banker',
					company: 'BMO Financial Group',
					city: 'Ottawa',
					prov: 'ON',
					date: 1602603150000,
					status: 'Interview',
				},
			],

		}
	},
	computed: {
		totalApplied() {
			return this.jobs.length;
		},
		lastSevenDaysApplied() {
			var total = 0;
			this.jobs.forEach( (item) => {
				total += ( item.date > (Date.parse(new Date) - 604800000) ) ? 1 : 0;
			}  )
			return total;
		},
	},
	methods: {
		addJob(newJob){ 
			this.jobs.push(newJob);
		},
		display(thedate) {
			const day = new Date(thedate);
			return week[day.getDay()] + ' ' + month[day.getMonth()] + ' ' + day.getDate() + ', ' + day.getFullYear()
		},
	},
};
</script>

<style>
.toolbar {
	z-index: 1;
}
.v-input__append-inner {
	margin: 0;
}
</style>

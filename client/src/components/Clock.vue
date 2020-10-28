<template>
	<v-card
		class="ma-4 py-5"
		width="400"
		height="400"
	>
	<ClockForm v-on:update-clock="updateTime"/>
	<v-row>
		<v-col cols="12">
			<h1>{{time}}</h1>
		</v-col>
	</v-row>
	<v-row>
		<v-col cols="12">
			<p>{{date}}</p>
		</v-col>
	</v-row>
	<v-row>
		<v-col cols="12">
		<div v-if="!sleep">
			<v-card-text>
				<div class="progcont">
					<v-progress-linear
						:value="percent"
						height="35"
						width="95%"
						class="ma-5"
						rounded
					>
						{{percent}}%
					</v-progress-linear>
				</div>
				<v-row>
					<v-col>
						<p>Start</p>
						<p>{{zeroPad(startH, 2)}}:{{zeroPad(startM, 2)}}</p>
						<p >{{sinceStart}}</p>
					</v-col>
					<v-col>
						<p>End</p>
						<p>{{zeroPad(endH, 2)}}:{{zeroPad(endM, 2)}}</p>
						<p>{{untilEnd}}</p>
					</v-col>
				</v-row>
			</v-card-text>
		</div>
		<div v-if="sleep">
			<v-card-text>
				<h1>SLEEPING</h1>
			</v-card-text>
		</div>
		</v-col>
	</v-row>
	</v-card>
</template>

<script>
import ClockForm from './ClockForm.vue'

const week = ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'];
const month = ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'];

export default {
	name: 'Clock',
	components: {
		ClockForm,
	},
	data () {
		return {
			date: '',
			time: '',
			startH: 7,
			startM: 0,
			endH: 23,
			endM: 0,
			percent: 0,
			sinceStart: '',
			untilEnd: '',
			sleep: false,
		}
	},
	mounted() {
		this.updateDate();
	},
	beforeDestroy() {
		window.clearTimeout(this.$options.timer);
	},
	methods: {
		updateDate() {
			const now = (new Date());
			const current = Date.parse(now);
			const start = Math.floor(( new Date() ).setHours(this.startH,this.startM,0)/1000)*1000;
			const end = Math.floor(( new Date() ).setHours(this.endH,this.endM,0)/1000) * 1000;
			this.time = this.zeroPad(now.getHours(), 2) + ':' + this.zeroPad(now.getMinutes(),2) + ':' + this.zeroPad(now.getSeconds(), 2);
			this.date = week[now.getDay()] + ' ' + month[now.getMonth()] + ' ' + this.zeroPad(now.getDate(), 2) + '/' + this.zeroPad(now.getFullYear(), 2);
			if ((end - current) < 0 || (current - start) < 0){
				this.sleep = true;	
			}
			else {
				this.sleep = false;
				this.percent = Math.floor((current-start)/(end-start)*1000)/10;
				this.sinceStart = this.msToTime(current - start);
				this.untilEnd = this.msToTime(end - current);

			}
			this.$options.timer = window.setTimeout(this.updateDate, 1000);
		},
		zeroPad(num, dig) {
			var zero = '';
			for (var i = 0; i < dig; i++){
				zero += '0';
			}
			return (zero + num).slice(-dig);
		},
		msToTime(duration) {
			var	seconds = Math.floor((duration / 1000) % 60);
			var	minutes = Math.floor((duration / (1000 * 60)) % 60);
			var	hours = Math.floor((duration / (1000 * 60 * 60)) % 24);

			hours = (hours < 10) ? "0" + hours : hours;
			minutes = (minutes < 10) ? "0" + minutes : minutes;
			seconds = (seconds < 10) ? "0" + seconds : seconds;
			return hours + "h " + minutes + "m " + seconds + "s ";
		},
		updateTime(item) {
			this.setTime(item);
		},
		setTime(item) {
			var start = item.start.split(':');
			var end = item.end.split(':');
			this.startH = parseInt(start[0], 10);
			this.startM = parseInt(start[1], 10);
			this.endH = parseInt(end[0], 10);
			this.endM = parseInt(end[1], 10);
			this.updateDate();
		},
	}
};
</script>

<style scoped>
* {
	font-family: 'Share Tech Mono', monospace;
	text-align: center;
	padding: 0;
	
}
h1 {
	font-size: 40pt;
}
.progcont {
	width: 90%;
}
</style>

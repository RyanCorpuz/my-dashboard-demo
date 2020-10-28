<template>
	<v-dialog
		v-model="dialog"
		persistent
		max-width="800px"
	>
		<template v-slot:activator="{ on, attrs }">
			<v-btn
				v-bind="attrs"
				v-on="on"
				icon
				dark
				x-small
				fab
				top
				right
				absolute
				class="mt-7"
			>
				<v-icon>mdi-pencil</v-icon>
			</v-btn>
		</template>
		<v-form
			ref="form"
		>
			<v-card>
				<v-card-title>
					Change Times
				</v-card-title>
				<v-card-text>
					<v-container>
							<v-row
								justify="space-around"
								align="center"
							>
								<v-col>
									<h2>Wake Up:</h2>
									<v-time-picker
									v-model="start"
									:max="end"
									></v-time-picker>
								</v-col>
								<v-col>
									<h2>Bed Time:</h2>
									<v-time-picker
									v-model="end"
									:min="start"
									></v-time-picker>
								</v-col>
							</v-row>
					</v-container>
				</v-card-text>
				<v-card-actions>
					<v-spacer></v-spacer>
					<v-btn
						color="blue darken-1"
						text
						v-on:click="dialog = false"
					>
					Close
					</v-btn>
					<v-btn
						color="blue darken-1"
						text
						:disabled="!start || !end"
						v-on:click="updateClock"
					>
					Update
					</v-btn>
				</v-card-actions>
			</v-card>
		</v-form>
	</v-dialog>
</template>

<script>
export default {
	name: 'ClockForm',
	data() {
		return {
			dialog: false,
			start: '',
			end: '',
		}
	},
	methods: {
		resetForm() {
			this.$refs.form.resetValidation();
			this.dialog = false;
		}
		,
		updateClock(e){
			e.preventDefault();
			const newClock = {
				start: this.start,
				end: this.end,
			};
			this.$emit('update-clock',newClock);
			this.resetForm();
		},
	},
};
</script>

<style>

</style>

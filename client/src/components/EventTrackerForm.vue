<template>
	<v-dialog
		v-model="dialog"
		persistent
		max-width="600px"
	>
		<template v-slot:activator="{ on, attrs }">
			<v-btn
				v-bind="attrs"
				v-on="on"
				dark
				small
				fab
				absolute
				bottom
				right
			>
				<v-icon>mdi-plus</v-icon>
			</v-btn>
		</template>
		<v-form
			ref="form"
		>
			<v-card>
				<v-card-title>
					Add Event
				</v-card-title>
				<v-card-text>
					<v-container>
						<v-row>
							<v-col
								cols="12"
							>
								<v-text-field
									v-model="description"
									label="Event Description"
									:rules="rules.text"
									required
								></v-text-field>
							</v-col>
							<v-col
								cols="12"
								sm="5"
							>
							</v-col>
						</v-row>
					</v-container>
				</v-card-text>
				<v-card-actions>
					<v-spacer></v-spacer>
					<v-btn
						color="blue darken-1"
						text
						v-on:click="resetForm"
					>
					Close
					</v-btn>
					<v-btn
						color="blue darken-1"
						text
						:disabled="!description"
						v-on:click="addNewEvent"
					>
					Add
					</v-btn>
				</v-card-actions>
			</v-card>
		</v-form>
	</v-dialog>
</template>

<script>
export default {
	name: 'EventTrackerForm',
	data() {
		return {
			dialog: false,
			description: '',
			rules: {
				text: [val => (val || '').length > 0 || 'This field is required'],
			},
		}
	},
	methods: {
		resetForm() {
			this.$refs.form.resetValidation();
			this.description = '';
			this.dialog = false;
		},
		addNewEvent(e){
			e.preventDefault();
			const newEvent = {
				description: this.description,
				dates: [],
			};
			this.$emit('add-event',newEvent);
			this.resetForm();
		},
	},
};
</script>

<style>

</style>

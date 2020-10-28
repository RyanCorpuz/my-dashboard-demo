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
					Job Hunt Logger
				</v-card-title>
				<v-card-text>
					<v-container>
						<v-row>
							<v-col
								cols="12"
							>
								<v-text-field
									v-model="job"
									label="Job Title"
									:rules="rules.text"
									required
								></v-text-field>
							</v-col>
							<v-col
								cols="12"
								sm="5"
							>
								<v-text-field
									v-model="company"
									label="Company"
									:rules="rules.text"
									persistent-hint
									required
								></v-text-field>
							</v-col>
							<v-col 
								cols="12"
								sm="4"
							>
								<v-text-field
									v-model="city"
									:rules="rules.text"
									label="City"
									required
								></v-text-field>
							</v-col>
							<v-col 
								cols="12"
								sm="3"
							>
								<v-select
									v-model="prov"
									:rules="rules.text"
									:items="provinces"
									label="Province"
									:maxlength="2"
									required
								></v-select>
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
						:disabled="!job || !company || !city || !prov"
						v-on:click="addNewJob"
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
	name: 'JobHuntTrackerForm',
	data() {
		return {
			dialog: false,
			job: null,
			company: null,
			city: null,
			prov: null,
			rules: {
				text: [val => (val || '').length > 0 || 'This field is required'],
			},
			provinces: ['AB', 'BC', 'MB', 'NB', 'NL', 'NT', 'NS', 'NU', 'ON', 'PE', 'QC', 'SK', 'YT'],
		}
	},
	methods: {
		resetForm() {
			this.$refs.form.resetValidation();
			this.dialog = false;
			this.job = null;
			this.company = null;
			this.city = null;
			this.prov = null;
		},
		addNewJob(e){
			e.preventDefault();
			const now = Date.parse(new Date());
			const newJob = {
				job: this.job,
				company: this.company,
				city: this.city,
				prov: this.prov,
				date: now,
				status: 'Applied',
			};
			this.$emit('add-job',newJob);
			this.resetForm();
		},
	},
};
</script>

<style>

</style>

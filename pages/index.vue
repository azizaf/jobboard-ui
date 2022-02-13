<template>
	<div class="container">
		<div class="row">
			<div class="col-12 py-4">
				<b-navbar toggleable="lg" type="light">
					<b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
					<b-collapse id="nav-collapse" is-nav>
						<b-navbar-nav class="ml-auto">
							<b-nav-form>
								<b-form-input size="sm" class="mr-sm-2" placeholder="Chercher un emploie"></b-form-input>
								<b-button size="sm" class="my-2 my-sm-0" type="submit">Chercher</b-button>
							</b-nav-form>
						</b-navbar-nav>
					</b-collapse>
				</b-navbar>
			</div>
		</div>

		<div class="row">
			<div class="col-12">
				<b-jumbotron header="Jobboard" lead="Your ultimate job board" />
				Page: {{ page }} / {{ pageCount }}  ( {{ pageJobs.length }} )
			</div>
		</div>

		<div class="row">
			<div class="col-12 col-md-6 mb-4" v-for="job in pageJobs" :key="job.link">
				<b-card :title="job.title" :sub-title="job.pubDate" tag="job" class="shadow border-0">
					<!-- <b-card-text>
						<p v-html="job.description+'...'"></p>
					</b-card-text> -->
					<b-button variant="primary" size="sm" :href="job.link">See job</b-button>
					<b-button variant="outline-primary" size="sm" :href="job.link">Add to favorite</b-button>
				</b-card>
			</div>
		</div>

		<div class="row">
			<b-pagination-nav :link-gen="linkGen" :number-of-pages="pageCount" use-router></b-pagination-nav>
		</div>
	</div>
</template>

<script>

export default {
	name: 'IndexPage',
	data() {
		return {
			jobs: [],
			jobsPerPage: 10
		}
	},
	async created() {
		this.jobs = await this.getJobs();
	},

	methods: {
		async getJobs() {
			try {
				let jobs = await this.$axios.$get("http://127.0.0.1:8000/jobs");
				return jobs
			}
			catch (e) {
				throw e;
			}
		},
		linkGen(pageNum) {
			return pageNum === 1 ? '?' : `?page=${pageNum}`
		}
	},
	computed: {
		pageCount( ) {
			return Math.ceil( this.jobs.length / this.jobsPerPage );
		},
		page() {
			return this.$route.query?.page ? this.$route.query.page : 1;
		},
		pageJobs() {
			let start = (this.page - 1) * this.jobsPerPage;
			let end = this.page * this.jobsPerPage;
			return this.jobs.slice(start, end);
		}
	}
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&family=Raleway:wght@700&display=swap');
html, body {
	font-family: "Poppins", sans-serif;
}
h1, h2, h3, h4 {
	font-family: "Raleway", sans-serif;
}

</style>
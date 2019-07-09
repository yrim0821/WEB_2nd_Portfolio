<template>
  <div class="py-3">
    <v-layout>

     <v-flex xs8>
       <a v-bind:href="repos.http_url_to_repo"><h2 v-line-clamp="1" class="font-weight-regular">{{repos.path_with_namespace}}</h2></a>
       <p class="subheading mb-1 grey--text text--darken-1 font-weight-light">{{repos.namespace.name}}</p>
     </v-flex>

   </v-layout>
  </div>
</template>

<script>
import Vue       from 'vue'
import lineClamp from 'vue-line-clamp'

Vue.use(lineClamp, {
  // plugin options
})
import GitlabService from '@/services/GitlabService'

export default {
	name: 'Repository',
	props: {
		repos: {type: null}
	},
	data() {
		return {
			stats: {}
		}
	},
  mounted() {
		this.drawStatGraph()
  },
	methods: {
		async drawStatGraph() {
			this.commits = await GitlabService.getCommits(this.repos.id)
		}
	}
}
</script>

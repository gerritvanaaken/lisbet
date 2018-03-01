<template>
	<div id="app">
		<bet-songs v-bind:songs="results.songs"></bet-songs>
	</div>
</template>

<script>
import Songs from './components/Songs.vue';
import axios from 'axios';

export default {
	name: 'app',
	components: {
		'bet-songs': Songs
	},
   methods: {
   	fetchSongs () {
   		axios.get('http://localhost:8080/src/assets/songs.json')
    			.then(response => {
    				this.results = response.data;
    		})
    			.catch(error => {
    				console.log(error);
    		})
   	}
   },
   created () {
      this.fetchSongs()
   },
	data () {
		return {
			results: {}
		}
	}
}
</script>

<style lang="scss">

* {
	margin: 0;
	padding: 0;
	font-size: 1em;
	font-family: inherit;
	font-weight: normal;
	list-style: none;
}

body {
	font-family: sans-serif;
	padding: 5%;
}

</style>

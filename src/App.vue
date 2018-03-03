<template>
	<div id="app" class="app">
		<div class="app__betarea">
			<bet-songs v-bind:songs="songdata.songs"></bet-songs>
			<bet-players v-bind:players="players" v-bind:songs="songdata.songs"></bet-players>
		</div>
	</div>
</template>

<script>
import Songs from './components/Songs.vue';
import Players from './components/Players.vue';
import axios from 'axios';

export default {
	name: 'app',
	components: {
		'bet-songs': Songs,
		'bet-players': Players
	},
   methods: {
   	fetchSongs () {
   		axios.get('http://localhost:8080/src/assets/songs.json')
    			.then(response => {
    				if (response.data.meta.stage !== 'init') {
    					response.data.songs.sort(function(a, b){
    						return b.points - a.points;
    					});
    				}
    				this.songdata = response.data;
    		})
    			.catch(error => {
    				console.log(error);
    		})
   	}
   },
   created () {
   	this.fetchSongs();
   	var songFetcher = setInterval(this.fetchSongs, 2000);
   },
	data () {
		return {
			songdata: {},
			players: []
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
	padding: 5%;
}

.app {
	font-family: sans-serif;
	&__betarea {
		display: flex;
	}
}

</style>

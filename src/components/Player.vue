<template>
	<article class="players__player player">

		<h3 class="player__name">{{ player.name }}</h3>
		<button @click="addRandomSong()">Add random song</button>
		<ul>
			<bet-song v-for="(song, index) in player.ranking" :song="song" :key="index" :index="index"></bet-song>
		</ul>
	</article>
</template>

<script>
import Song from './Song.vue';
export default {
	name: 'player',
	components: {
		'bet-song': Song
	},
	props: {
		songs: {
			type: Array
		},
		player: {
			type: Object
		}
		
	},
	methods: {
		addRandomSong() {
			// does not hot-reference the song. Unfortunately. Can we fix that?
			this.player.ranking.push(this.songs[Math.floor(Math.random() * 26)]);
		}
	}
}
</script>

<style lang="scss">

.player {
	border: 1px solid #aaa;
	box-shadow: 0 0 1rem rgba(0,0,0,0.2);
	margin: 1em 0;
	border-radius: .2rem;
	padding: 1rem;
}

</style>

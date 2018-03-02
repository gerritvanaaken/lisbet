<template>
	<article class="players__player player">

		<h3 class="player__name">{{ player.name }}</h3>
		<ul>
			<bet-song v-for="(rankedcountry, index) in player.ranking" :index="index" :song="getSongByCountry(rankedcountry)"></bet-song>
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
	data () {
		return { }
	},
	methods: {
		getSongByCountry: function (country) {
			console.log(songs);
			return {};
			songs.forEach(function(song) {
				if (onesong.country === country) {
					return song;
				}
			});
			return {};
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

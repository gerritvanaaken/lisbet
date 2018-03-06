<template>
	<article class="players__player player">

		<h3 class="player__name" @click="toggleNameChange">{{ player.name }}</h3>
		<input type="text" v-model="player.name" v-if="namechange" @blur="toggleNameChange" autofocus />
		<h2>Rank Difference: {{ rankDifference }}</h2>

		<draggable v-model="player.ranking" :options="{group: {name: 'playerlist', put: checkForDuplicates, pull: false}, animation: 300}">
			<transition-group name="flip-list" tag="ul" class="player__songlist">
				<bet-song v-for="(song, index) in player.ranking" :song="song" :key="song.country" :index="index"></bet-song>
			</transition-group>
		</draggable>
	</article>
</template>

<script>
import Song from './Song.vue';
import draggable from 'vuedraggable'

export default {
	name: 'player',
	components: {
		draggable,
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
	data() {
		return {
			namechange: false
		}
	},
	methods: {
		toggleNameChange() {
			if (this.namechange) {
				this.namechange = false;
			} else {
				this.namechange = true;
			}
		},
		checkForDuplicates() {
			var draggingCountry = this.$root.$data.draggingCountry;
			var output = true;
			this.player.ranking.forEach(function(song){
				if (song.country === draggingCountry) {
					output = false;
				}
			});
			return output;
		}
	},
	computed: {
		rankDifference() {
			var rankDiff = 0;
			var v = this;
			v.songs.forEach(function(song, i){
				v.player.ranking.forEach(function(songGuessed, j) {
					if (song.country === songGuessed.country) {
						rankDiff += Math.abs(i-j);
					}
				});
				
			});
			return rankDiff;
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
	&__songlist {
		min-height: 5rem;
	}
}

</style>

<template>
	<article class="players__player player">

		<h3 class="player__name" @click="toggleNameChange">{{ player.name }}</h3>
		<input type="text" v-model="player.name" v-if="namechange" @blur="toggleNameChange" autofocus />
		<h2 v-if="meta.stage !== 'init'">Rank Difference: {{ rankDifference }}</h2>

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
		},
		meta: {
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
	width: 18vw;
	min-width: 10rem;
	max-width: 30rem;
	transition: all .4s;
	border-left: 1px solid #aaa;
	padding: 1rem;
	display: flex;
	flex-direction: column;
	align-items: stretch;
	justify-content: stretch;
	height: calc(100% - 1.5rem);
	&:last-child {
		border-right: 1px solid #aaa;
	}
	&__name {
		font-weight: 700;
		color: #fff;
	}
	& > div {
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: stretch;
		justify-content: stretch;
	}
	&__songlist {
		background: rgba(255,255,255,.15);
		border-radius: .2rem;
		margin-top: .5rem;
		height: 100%;
		position: relative;
		&:empty:after {
			content: "Drag songs here!";
			display: block;
			position: absolute;
			top: 20vh;
			left: 20%;
			right: 20%;
			color: rgba(255,255,255,.5);
			font-weight: 100;
			font-size: 2rem;
			text-align: center;

		}
	}
	
}

</style>

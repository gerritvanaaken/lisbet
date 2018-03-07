<template>
	<article class="players__player player">

		<header class="player__header">
			<input class="player__name" type="text" v-model="player.name" />
			<div class="player__score" v-if="meta.bettingLocked && player.ranking.length === songs.length">{{ score }}</div>
			<button v-if="!meta.bettingLocked" class="player__button player__button--import" @click="importSongs" title="Copy songlist">=</button>
			<button v-if="!meta.bettingLocked"  class="player__button player__button--delete" @click="deletePlayer" title="Remove this player">&times;</button>
		</header>

		<draggable v-model="player.ranking" :options="{group: {name: 'playerlist', put: checkForDuplicates, pull: false, revertClone: true}, animation: 300}">
			<transition-group name="flip-list" tag="ul" class="player__songlist">
				<bet-song v-for="(song, index) in player.ranking" :song="song" :key="song.country" :index="index" :locked="meta.bettingLocked" @delete="deleteSong(index)"></bet-song>
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
		},
		deletePlayer() {
			this.$emit('delete');
		},
		deleteSong(index) {
			this.player.ranking.splice(index, 1);
		},
		importSongs() {
			var mysongs = this.player.ranking;
			this.songs.forEach(function(original) {
				var importsong = true;
				mysongs.forEach(function(mysong) {
					if (original.country === mysong.country) {
						importsong = false;
					}
				});
				if (importsong) {
					mysongs.push(original);
				}
			});
		}
	},
	computed: {
		score() {
			var rankDiff = 0;
			var v = this;
			v.songs.forEach(function(song, i){
				v.player.ranking.forEach(function(songGuessed, j) {
					if (song.country === songGuessed.country) {
						rankDiff += Math.abs(i-j);
					}
				});
				
			});
			return 1000 - rankDiff;
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
	padding: 1rem .5rem 0 1rem;
	display: flex;
	flex-direction: column;
	align-items: stretch;
	justify-content: stretch;
	height: calc(100% - 1.5rem);
	&:first-child {
		border-left: 1px solid #aaa;
	}
	&__header {
		position: relative;
		display: flex;
		width: 100%;
		justify-content: flex-end;
	}
	&__button {
		background: #fff;
		border: none;
		display: block;
		width: 1em;
		line-height: .95;
		color: #333;
		border-radius: 50%;
		font-weight: 100;
		font-size: 1.5rem;
		cursor: pointer;
		outline: none;
		margin-left: .4rem;
		&:hover, &:focus {
			background: rgba(255,255,255,.8);
		}
	}
	&__score {
		background: #c09;
		color: #fff;
		font-size: .9rem;
		padding: .08em .5em;
		border-radius: 2em;
	}
	&__name {
		font-weight: 700;
		color: #fff;
		outline: none;
		border: none;
		background: none;
		padding: 0;
		margin: 0;
		line-height: 1;
		width: 60%;
		position: absolute;
		left: 0;
		&:focus {
			box-shadow: inset 0 0 0 1px rgba(255,255,255,.5);
			padding-left: .5em;
		}

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

<template>
	<section class="players">
		<h2 class="players__headline">Who will win?</h2>
		<button @click="addPlayer()">Add New Player</button>
		<div class="players__list">
			<bet-player v-for="(player, index) in players" :key="index" :player="player" :songs="songs"></bet-player>
		</div>
	</section>
</template>

<script>
import Player from './Player.vue';
export default {
	name: 'players',
	components: {
		'bet-player': Player
	},
	props: {
		players: {
			type: Array
		},
		songs: {
			type: Array
		}
	},
	data () {
		return {
	
		}
	},
	methods: {
		addPlayer() {
			this.players.push({
				name: 'New Player',
				ranking: []
			})
		}
	}
}
</script>

<style lang="scss">
.players {
	margin: 0 0 0 3rem;
	&__list {
		display: flex;
	}
}

</style>

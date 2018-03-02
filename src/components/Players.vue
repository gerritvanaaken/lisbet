<template>
	<section class="players">
		<h2 class="players__headline">Who will win?</h2>
		<div class="players__list">
			<bet-player v-for="player in players" :player="player" :songs="songs"></bet-player>
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

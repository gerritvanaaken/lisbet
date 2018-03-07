<template>
	<section class="players">
		<bet-player v-for="(player, index) in players" :key="index" :player="player" :songs="songs" :meta="meta"></bet-player>
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
		},
		meta: {
			type: Object
		}
	}
}
</script>

<style lang="scss">
.players {
	margin: 0 0 0 1rem;
	display: flex;
	justify-content: stretch;
	overflow-x: auto;
	-webkit-overflow-scrolling: touch;

}

</style>

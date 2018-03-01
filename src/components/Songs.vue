<template>
	<div class="songs">
		<h2 class="songs__headline">Liste der Songs</h2>
		<transition-group name="flip-list" tag="ul" class="songs__list">
			<bet-song v-for="(song, index) in songs" v-bind:song="song" v-bind:key="song.country" v-bind:index="index" ></bet-song>
		</transition-group>
	</div>
</template>

<script>
import Song from './Song.vue';
export default {
	name: 'songs',
	components: {
		'bet-song': Song
	},
	props: {
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
.songs {
	max-width: 20em;
	
}
.flip-list-move {
  transition: transform 1s;
}
</style>

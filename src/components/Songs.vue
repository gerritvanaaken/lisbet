<template>
	<div class="songs">
		<h2 class="songs__headline">Liste der Songs</h2>
		<ul class="songs__list">
			<li class="songs_item" v-for="song in songs">
				<bet-song v-bind:song="song"></bet-song>
			</li>
		</ul>
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
</style>

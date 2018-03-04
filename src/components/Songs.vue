<template>
	<div class="songs">
		<h2 class="songs__headline">Liste der Songs</h2>
		<draggable v-model="songs" class="dragArea" @sort="alert('sdkj')" :options="{group: {name: 'songs', pull: 'clone'}}">
			<transition-group name="flip-list" tag="ul" class="songs__list">
				<bet-song v-for="(song, index) in songs" :song="song" :key="song.country" :index="index" ></bet-song>
			</transition-group>
		</draggable>
	</div>
</template>

<script>
import Song from './Song.vue';
import draggable from 'vuedraggable'

export default {
	name: 'songs',
	components: {
		draggable,
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

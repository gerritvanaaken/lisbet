<template>
	<div class="songs">
		<h2 class="songs__headline">Participants {{ meta.showtype }}</h2>
		<draggable :list="songs" @start="globalStoreCandidate" @end="globalRemoveCandidate" :options="{sort: false, group: {name: 'songs', pull: 'clone', put: false}, animation: 300}">
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
		},
		meta: {
			type: Object
		}

	},
	methods: {
		globalStoreCandidate(dragitem) {
			// global storage :-( maybe use vuex later, seems over-engineered, though
			this.$root.$data.draggingCountry = dragitem.clone.getElementsByClassName('song__countrycode')[0].innerText;
		},
		globalRemoveCandidate() {
			this.$root.$data.draggingCountry = '';
		}
	}
}
</script>

<style lang="scss">
.songs {
	max-width: 20rem;
	&__headline {
		color: #fff;
	}
	.flip-list-move {
		transition: transform .7s;
	}
}

</style>

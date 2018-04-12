<template>
	<div class="songs">
		<h2 class="songs__headline" v-if="!meta.bettingLocked">Participants: {{ meta.showtype }}</h2>
		<h2 class="songs__headline" v-if="meta.bettingLocked && !meta.finished">Voting in progress …</h2>
		<h2 class="songs__headline" v-if="meta.finished">Results: {{ meta.showtype }}</h2>
		<draggable :list="songs" @start="globalStoreCandidate" @end="globalRemoveCandidate" :options="{sort: false, group: {name: 'songs', pull: 'clone', put: false}, animation: 300}" class="songs__scroller">
			<transition-group name="flip-list" tag="ul" class="songs__list">
				<bet-song v-for="(song, index) in songs" :song="song" :key="song.country" :index="index" :locked="meta.bettingLocked" :finished="meta.finished"></bet-song>
			</transition-group>
		</draggable>
		<div class="songs__scrollers scrollers">
			<button class="scrollers__button scrollers__button--up" @click="scrollUp">hoch</button>
			<button class="scrollers__button scrollers__button--down" @click="scrollDown">runter</button>
		</div>
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
		},
		scrollUp() {
			var scroller = this.$el.getElementsByClassName('songs__scroller')[0];
			var offset = scroller.scrollTop;
			scroller.scrollTop = offset - 20;
		},
		scrollDown() {
			var scroller = this.$el.getElementsByClassName('songs__scroller')[0];
			var offset = scroller.scrollTop;
			scroller.scrollTop = offset + 20;
		}
	}
}
</script>

<style lang="scss">
.songs {
	position: relative;
	margin: 1rem 0 1rem 1rem;
	width: 320px;
	@media only screen and (max-width: 550px) {
		width: 45vw;
	}
	&__headline {
		color: #fff;
		font-weight: 700;
		white-space: nowrap;
		width: 100%;
		overflow-x: hidden;
		text-overflow: ellipsis;
	}
	.flip-list-move {
		transition: transform .7s;
	}
	&__scroller {
		margin-top: .5rem;
		height: calc(100vh - 7rem);
		overflow-y: auto;
		-webkit-overflow-scrolling: touch;
	}
	&__list {
		margin-right: .5rem;
		@media only screen and (min-width: 550px) {
			margin-right: 1rem;
		}
	}
	&__scrollers {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 1rem;
		background: red;
		padding: .5rem;
		text-align: center;
	}
}

</style>

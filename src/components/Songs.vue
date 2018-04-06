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
	position: relative;
	width: 320px;
	margin: 1rem 0 1rem 1rem;
	@media only screen and (max-width: 550px) {
		width: 45vw;
	}
	&:before {
		border-radius: .2rem;
		content: '';
		position: absolute;
		background-image: 
			linear-gradient(rgba(#fff,.3) .1em, transparent .1em);
		background-size: 100% 1.2em;
		width: 1.6rem;
		bottom: 0;
		border-left: 1px solid rgba(#fff,.4);
		border-right: 1px solid rgba(#fff,.4);
		top: 1.8rem;
		right: 0;
	}
	&:after {
		content: '« »';
		position: absolute;
		font-size: 2rem;
		color: #fff;
		top: 50%;
		font-weight: 700;
		right: .62rem;
		text-transform: uppercase;
		letter-spacing: .1em;
		transform-origin: 100% 50%;
		transform: rotate(90deg) translateX(50%);
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
		margin-right: 2rem;
	}
}

</style>

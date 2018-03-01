<template>
	<li class="songs__song song">
		<div class="song__country">
			<img class="song__flag" v-bind:src="'./src/assets/flags/' + song.country + '.svg'" width="30" height="20" />
			<span class="song__countrycode">{{ song.country }}</span>
		</div>
		<h3 class="song__title">{{ song.title }}</h3>
		<div class="song_artist">{{ song.artist }}</div>
		<div class="song__order">{{ song.order }}</div>
		<div class="song__points">{{ song.points }}</div>
		<div class="song__rank">{{ index + 1 }}</div>
	</li>
</template>

<script>
export default {
	name: 'song',
	props: {
		song: {
			type: Object
		},
		index: {
			type: Number
		}
	},
	data () {
		return {
	
		}
	}
}
</script>

<style lang="scss">

.song {
	border: 1px solid #aaa;
	box-shadow: 0 0 1rem rgba(0,0,0,0.2);
	margin: 1em 0;
	border-radius: .2rem;
}

</style>

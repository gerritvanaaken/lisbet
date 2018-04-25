<template>
	<div id="app" class="app">
		<header class="app__header">
			<h1 class="app__headline">#lisbet</h1>
			<button v-if="!songdata.meta.bettingLocked" class="app__button" @click="addPlayer()">Add New Player</button>
			<div class="app__subline">The betting game for Lisbon 2018</div>
		</header>
		<div class="app__betarea">
			<bet-songs :songs="songdata.songs" :meta="songdata.meta"></bet-songs>
			<bet-players :players="players" :songs="songdata.songs" :meta="songdata.meta"></bet-players>
		</div>
	</div>
</template>

<script>
import Songs from './components/Songs.vue';
import Players from './components/Players.vue';
import axios from 'axios';

var draggingCountry = true;

export default {
	name: 'app',
	components: {
		'bet-songs': Songs,
		'bet-players': Players
	},
	methods: {
		fetchSongs () {
			axios.get('/src/assets/songs.json').then(response => {
				if (response.data.meta.bettingLocked) {
					response.data.songs.sort(function(a, b){
						return b.points - a.points;
					});
				} else {
					response.data.songs.sort(function(a, b){
						return a.order - b.order;
					});
				}
				this.songdata = response.data;
			}).catch(error => {
				console.log(error);
			})
		},
		addPlayer() {
			this.players.push({
				name: 'Player '+ (this.players.length + 1),
				ranking: []
			})
		}
	},
	created () {
		this.fetchSongs();
		var songFetcher = setInterval(this.fetchSongs, 2000);
	},
	mounted () {
		if (localStorage.getItem('players')) {
			this.players = JSON.parse(localStorage.getItem('players'));
		}
	},
	data () {
		return {
			songdata: {
				"meta": {
					"formatversion": "1",
					"day": "XXXX-XX-XX",
					"showtype": "XXX",
					"bettingLocked": false,
					"finished": false
				},
				"songs": []
			},
			players: []
		}
	},
	watch: {
		players: {
			handler() {
				localStorage.setItem('players', JSON.stringify(this.players))
			},
			deep: true
		}
	}
}
</script>

<style lang="scss">

@font-face {
	font-family: 'robotoslab';
	src: url('./assets/fonts/RobotoSlab-Thin-webfont.woff');
	font-weight: 100;
	font-style: normal;
}
@font-face {
	font-family: 'robotoslab';
	src: url('./assets/fonts/RobotoSlab-Light-webfont.woff');
	font-weight: 200;
	font-style: normal;
}
@font-face {
	font-family: 'robotoslab';
	src: url('./assets/fonts/RobotoSlab-Regular-webfont.woff');
	font-weight: 400;
	font-style: normal;
}
@font-face {
	font-family: 'robotoslab';
	src: url('./assets/fonts/RobotoSlab-Bold-webfont.woff');
	font-weight: 700;
	font-style: normal;
}



* {
	margin: 0;
	padding: 0;
	font-size: 1em;
	font-family: inherit;
	font-weight: normal;
	list-style: none;
}

body, html {
	overflow: hidden;
	position: fixed;
	top: 0;
	right: 0;
	left: 0;
	bottom: 0;
}

body {
	background: #346;
}

.app {
	font-family: 'robotoslab', sans-serif;
	display: flex;
	flex-direction: column;
	height: 100%;
	&__header {
		padding: .5rem 1rem;
		background: rgba(0,0,0,0.5);
		color: #fff;
		display: flex;
		align-items: center;
		justify-content: space-between;
		width: calc(100% - 2rem);
	}
	&__headline {
		text-transform: uppercase;
		font-weight: 100;
		letter-spacing: .1em;
		font-size: 2rem;
	}
	&__subline {
		text-transform: uppercase;
		letter-spacing: .1em;
		@media only screen and (max-width: 900px) {
			display: none;
		}
	}
	&__button {
		background: none;
		border: 1px solid #fff;
		border-radius: .3rem;
		color: #fff;
		font-weight: 200;
		padding: .2em 1em;
		cursor: pointer;
		outline: none;
		@media only screen and (max-width: 400px) {
			width: 3.2rem;
			overflow: hidden;
			white-space: nowrap;
		}
		&:before {
			content: '+';
			background: #fff;
			color: #333;
			text-align: center;
			width: 1em;
			line-height: 1;
			margin-right: .5em;
			border-radius: 50%;
			display: inline-block;
			@media only screen and (max-width: 400px) {
				margin-right: 3rem;
			}
		}
		&:focus, &:hover {
			background: rgba(255,255,255,.2);
		}
	}
	&__betarea {
		height: 100%;
		display: flex;
		align-items: stretch;
	}
}

.scrollers {
	position: absolute;
	bottom: 0;
	left: 0;
	right: 1rem;
	@media only screen and (max-width: 550px) {
		right: .5rem;
	}
	background: rgba(0,0,0,0.5);
	padding: .5rem;
	text-align: center;
	border-top-left-radius: .3rem;
	border-top-right-radius: .3rem;
	&__button {
		position: relative;
		-webkit-appearance: none;
		background: none;
		border: 1px solid #fff;
		display: inline-block;
		padding: .5rem;
		margin: 0 .2rem;
		color: #fff;
		border-radius: .2rem;
		line-height: 1;
		cursor: pointer;
		text-indent: -10em;
		overflow: hidden;
		width: 2.5rem;
		&--up {
			transform: rotate(180deg);
		}
		&:focus, &:hover {
			outline: none;
			background: rgba(255,255,255,.2);
		}
		&:before, &:after {
			content: '';
			width: .75rem;
			height: 1px;
			background: #fff;
			transform: rotate(45deg);
			position: absolute;
			top: 1rem;
			left: .55rem;
		}
		&:after {
			right: .55rem;
			left: auto;
			transform: rotate(-45deg);
		}
	}
}

</style>

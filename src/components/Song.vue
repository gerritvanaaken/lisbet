<template>
	<li class="songs__song song">
		<div class="song__country">
			<img class="song__flag" v-bind:src="'./src/assets/flags/' + song.country + '.svg'" width="30" height="20" />
			<span class="song__countrycode">{{ song.country }}</span>
		</div>
		<h3 class="song__title">{{ song.title }}</h3>
		<div class="song__artist">{{ song.artist }}</div>
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
	}
}
</script>

<style lang="scss">

.song {
	position: relative;
	padding: .5rem;
	background: linear-gradient(135deg, rgba(255,255,255,.9) 0, rgba(255,255,255,.4) 100%);
	box-shadow: 0 0 1rem rgba(0,0,0,0.5);
	margin: 0 0 .5rem 0;
	border-radius: .2rem;
	cursor: e-move;
	cursor: e-resize;
	&:focus, &:hover {
		background: linear-gradient(135deg, #fff 0, #aaa 100%);
	}
	.player & {
		cursor: ns-resize;
	}
	&__country {
		display: flex;
		flex-direction: column;
		float: right;
		margin: 0 0 0 .8rem;
		text-align: center;
		.player & {
			float: left;
			margin: 0 .8rem 0 0;
		}
	}
	&__flag {
		width: 36px;
		height: auto;
	}
	&__countrycode {
		&:before {
			content: '#';
		}
		display: block;
		margin-top: .1rem;
		font-size: .8rem;
		font-weight: bold;
	}
	&__title {
		font-weight: 200;
		
		text-overflow: ellipsis;
		overflow: hidden;
		width: calc(100% - 3.5rem);
		white-space: nowrap;
	}
	&__artist {
		text-overflow: ellipsis;
		overflow: hidden;
		width: calc(100% - 3.5rem);
		white-space: nowrap;
		font-weight: 400;
	}
	
	&__order, &__points {
		display: none;
	}
	&__rank {
		font-weight: 100;
		position: absolute;
		top: .12rem;
		right: 3.5rem;
		letter-spacing: -.05em;
		z-index: 0;
		font-size: 3.2rem;
		line-height: 1;
		color: rgba(0,0,0,0.2);
		.player & {
			display: none;
		}
	}

}

</style>

<script lang="ts">
	import uid from "uid";

	const route = location.pathname.slice(1);
	const chatURL = `${location.origin}/${uid()}`;
</script>

<header>
	<h1>Svelte Chat</h1>
	<p>By Berkin AKKAYA</p>
</header>

{#if route == ''}
	<p id="tutorial">Share this URL with people, then visit it:</p>
	<a id="chatLink" href={chatURL}>{chatURL}</a>
{:else}
	<p style="font-size: 2em">
		<b>Your Chat ID:</b>
		<span style="text-decoration: underline">{route}</span>
	</p>
	<p style="margin-top: 50px">This Page Is Under Development!</p>
{/if}

<style lang="scss">
	header {
		margin: 100px 0;

		p {
			opacity: 0.5;
			text-align: center;
			margin-top: 10px;
		}
	}
	#tutorial {
		margin-bottom: 25px;
		font-size: 1.5em;
		opacity: 0.8;
	}
	#chatLink {
		color: white;
		font-size: 2em;
		text-decoration: none;
		padding: 30px;
		border-radius: 10px;
		border: 1px solid rgba($color: #fff, $alpha: 0.4);
		box-shadow: 0 0 5px rgba($color: #000, $alpha: 0);
		color: rgba($color: #fff, $alpha: 0.8);

		&:hover {
			background-color: rgba($color: #fff, $alpha: 0.1);
			border: 1px solid rgba($color: #fff, $alpha: 1);
			box-shadow: 0 0 10px rgba($color: #000, $alpha: 0.5);
			color: white;
		}
	}
</style>

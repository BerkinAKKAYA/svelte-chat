<script lang="ts">
	import uid from "uid";
	import ChatPanel from "./ChatPanel.svelte";

	const route = location.href.split("/").slice(-1)[0];
	const chatURL = `${location.origin}/${uid()}`;

	$: cleanURL = chatURL.replace("http://", "").replace("https://", "");
</script>

<header>
	<a href="/"><h1>Svelte Chat</h1></a>
	<p>By Berkin AKKAYA</p>
</header>

{#if route == ''}
	<p id="tutorial">Share this URL with a friend, then visit it:</p>
	<a id="chatLink" href={chatURL}>{cleanURL}</a>
{:else}
	<ChatPanel id={route} />
{/if}

<style lang="scss">
	header {
		margin: 100px 0;

		a {
			color: white;
			text-decoration: none;
		}
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
		text-align: center;
		padding: 0 50px;
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

		@media (max-width: 1000px) {
			font-size: 2.8vw;
			padding: 0;
			border: none;
		}

		&:hover {
			background-color: rgba($color: #fff, $alpha: 0.1);
			border: 1px solid rgba($color: #fff, $alpha: 1);
			box-shadow: 0 0 10px rgba($color: #000, $alpha: 0.5);
			color: white;
		}
	}
</style>

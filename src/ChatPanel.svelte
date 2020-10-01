<script lang="ts">
	export let id = "";
</script>

<h2 style="margin-bottom: 50px">THIS PAGE IS UNDER DEVELOPMENT!</h2>

<main>
	<div class="message received">Hi!</div>
	<div class="message received">How are you?</div>
	<div class="message sent">Fine, you?</div>
	<div class="message received">Me too...</div>
	<input type="text" placeholder="Type Something..." />
</main>
<p style="margin-top: 150px; opacity: .5">chat id: {id}</p>

<style lang="scss">
	main {
		width: 85vw;
		max-width: 600px;
	}
	.message {
		margin: 5px 0;
		padding: 15px;
		font-size: 1.5em;
		border-radius: 5px;

		&.sent {
			text-align: right;
			background-color: rgba($color: white, $alpha: 0.35);
		}
		&.received {
			text-align: left;
			background-color: rgba($color: white, $alpha: 0.15);
		}
	}
	input[type="text"] {
		width: 100%;
		padding: 20px;
		margin-top: 10px;
		outline: none;
		border: 1px solid rgba($color: white, $alpha: 0.5);
		background-color: rgba($color: white, $alpha: 0.1);
		border-radius: 10px;
		font-size: 1.2em;

		&,
		&::placeholder {
			color: rgba($color: white, $alpha: 0.5);
			transition: color 0.2s ease;
		}
		&:hover {
			border: 1px solid rgba($color: white, $alpha: 0.6);

			&,
			&::placeholder {
				color: rgba($color: white, $alpha: 0.75);
			}
		}
		&:focus {
			border: 1px solid rgba($color: white, $alpha: 0.9);

			&,
			&::placeholder {
				color: rgba($color: white, $alpha: 1);
			}
		}
	}
</style>

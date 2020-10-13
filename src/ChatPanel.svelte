<script lang="ts">
	export let id = "";

	import uid from "uid";
	import { formatDistance } from "date-fns";
	import firebase from "firebase/app";
	import "firebase/firebase-firestore";

	const config = {
		apiKey: "AIzaSyDIq-xr_oPNNTkfSCACXelShMxRsrljHss",
		authDomain: "svelte-chat-berkinakkaya.firebaseapp.com",
		databaseURL: "https://svelte-chat-berkinakkaya.firebaseio.com",
		projectId: "svelte-chat-berkinakkaya",
		storageBucket: "svelte-chat-berkinakkaya.appspot.com",
		messagingSenderId: "173137532990",
		appId: "1:173137532990:web:2538e32e74e4c081dd596f",
		measurementId: "G-G01N1MVE6P"
	};
	firebase.initializeApp(config);

	let clientId = localStorage.getItem("clientId") || false;
	if (!clientId) {
		const newClientId = uid();
		localStorage.setItem("clientId", newClientId);
		clientId = newClientId;
	}

	const db = firebase.firestore();
	const doc = db.collection("chats").doc(id);
	let data = {};

	// Fetch The Data for Initialization (Promise)
	const fetchDoc = doc.get().then((ss) => {
		const fetchedData = ss.data();

		if (!fetchedData) {
			doc.set({});
			data = {};
		} else {
			data = fetchedData;
		}

		return data;
	});

	doc.onSnapshot((ss) => (data = ss.data() || {}));

	// Show & Hide "Created At" Indicators
	document.addEventListener("click", (e) => {
		const createdAtElements = document.getElementsByClassName("createdAt");
		for (const el of createdAtElements) {
			(el as HTMLElement).classList.remove("visible");
		}

		const clickedElement = e.target as HTMLElement;
		if (clickedElement && clickedElement.classList[0] === "content") {
			const parent = clickedElement.parentNode;
			const element = parent.querySelector(".createdAt") as HTMLElement;
			element.classList.add("visible");
		}
	});

	// Keep the data always sorted
	$: {
		const sortedData = {};
		Object.keys(data)
			.sort()
			.forEach((key) => {
				sortedData[key] = data[key];
			});
		data = sortedData;
	}

	function SendMessage(e) {
		const val = e.target.value;
		e.target.value = val.replace("  ", " ");

		if (e.key === "Enter" && val) {
			data[Date.now()] = {
				message: val,
				sender: clientId,
			};
			e.target.value = "";
			doc.set(data);
		}
	}

	function CopyURL() {
		navigator.clipboard.writeText(location.href);

		const button = document.getElementById("share");
		button.innerHTML = "Copied!";
		button.style.backgroundColor = "rgba(0, 0, 140, 1)";
		button.style.pointerEvents = "none";

		setTimeout(() => {
			button.innerHTML = "Copy Chat URL";
			button.style.backgroundColor = "rgba(0, 0, 140, 0.5)";
			button.style.pointerEvents = "";
		}, 1000);
	}
</script>

<main>
	{#await fetchDoc}
		<p class="loading">Loading...</p>
	{:then d}
		{#if Object.keys(data).length === 0}
			<div class="loading">No Chat History Yet</div>
		{:else}
			<div id="messages">
				{#each Object.entries(data) as [key, msg]}
					<div
						class="message {msg['sender'] === clientId ? 'sent' : 'received'}">
						<div class="content">{msg['message']}</div>
						<div class="createdAt">
							{formatDistance(parseInt(key), Date.now())}
							ago
						</div>
					</div>
				{/each}
			</div>
		{/if}
	{/await}
	<input
		type="text"
		placeholder="Type Something..."
		on:keydown={SendMessage} />

	<button id="share" on:click={CopyURL}>Copy Chat URL</button>

	<div class="warning">
		To test the app and chat with yourself, copy the Chat URL
		and open it in a <b>private</b> tab.
	</div>
</main>

<style lang="scss">
	main {
		width: 85vw;
		max-width: 600px;
	}
	.loading {
		text-align: center;
		margin-bottom: 50px;
	}
	#messages {
		max-height: 500px;
		overflow-y: scroll;
		display: flex;
		flex-direction: column;

		.message {
			display: flex;
			flex-direction: column;
			margin: 5px 10px;
			position: relative;
			cursor: pointer;

			&.sent,
			&.sent .content {
				align-self: flex-end;
				text-align: right;
			}
			&.received,
			&.received .content {
				align-self: flex-start;
				text-align: left;
			}
			&.sent .content {
				background-color: rgba($color: white, $alpha: 0.35);
			}
			&.received .content {
				background-color: rgba($color: white, $alpha: 0.15);
			}
			.content {
				padding: 15px;
				font-size: 1.5em;
				border-radius: 5px;
				border: 2px solid rgba($color: white, $alpha: 0);

				&:hover {
					border-color: rgba($color: white, $alpha: 0.5);
				}
			}

			.createdAt {
				overflow: hidden;
				opacity: 0.8;
				margin-top: 5px;

				&:not(.visible) {
					transform: scale(0);
					height: 0;
				}
			}
		}

		&::-webkit-scrollbar {
			width: 5px;
		}
		&::-webkit-scrollbar-track {
			border-radius: 10px;
		}
		&::-webkit-scrollbar-thumb {
			background-color: rgba($color: #000000, $alpha: 0.1);
			border-radius: 10px;
		}
		&::-webkit-scrollbar-thumb:hover {
			background-color: rgba($color: #000000, $alpha: 0.2);
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
	#share {
		position: relative;
		left: 50%;
		transform: translateX(-50%);
		margin-top: 50px;
		padding: 15px 25px;
		border: none;
		outline: none;
		border-radius: 10px;
		color: white;
		background-color: rgba(0, 0, 0, 0.5);
		cursor: pointer;

		&:hover {
			background-color: rgba(0, 0, 140, 1) !important;
		}
	}

	.warning {
		margin: 50px 0;
		text-align: center;
		opacity: 0.8;
	}
</style>

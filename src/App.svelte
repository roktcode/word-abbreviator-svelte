<script>
	import { onMount } from "svelte";

	let inputRef;
	let notificationsRef;

	let text = "";
	let result = "";
	let notification = "";

	let textCases = ["Original", "Upper Case", "Lower Case"];
	let textCase = textCases[0];

	function getFirstLetters(source, c) {
		const result = source
			.trim()
			.split(" ")
			.map((word) => word[0])
			.join("");

		switch (c) {
			case "Lower Case":
				return result.toLowerCase();
			case "Upper Case":
				return result.toUpperCase();
			default:
				return result;
		}
	}

	function notify(message) {
		notification = message;
		notificationsRef.classList.remove("hidden");

		setTimeout(() => {
			notificationsRef.classList.add("hidden");
		}, 1000);
	}

	async function copyResult() {
		console.log("copied!");

		try {
			await navigator.clipboard.writeText(result);
			console.log(`Copied successfully!`);

			notify("Copied!");
		} catch (e) {
			console.error(e);
		}
	}

	function clearText() {
		text = "";
		inputRef.focus();
	}

	$: {
		result = getFirstLetters(text, textCase);
	}

	$: console.log(textCase);

	onMount(() => {
		inputRef.focus();
	});
</script>

<div class="container">
	<div class="intro">
		<h2>Welcome to Abbreviator ✍🏼</h2>
		<p>Start typing, and get the first letters of each word!</p>
	</div>

	<div class="input">
		<input
			type="text"
			bind:value={text}
			bind:this={inputRef}
			placeholder="Enter text here"
		/>
		{#if text}
			<button on:click={clearText}>❌</button>
		{/if}
	</div>
	<!-- 
	<div class="submit">
		<button on:click={getFirstLetters} >Get Result</button>
	</div> -->

	<div class="options">
		<div class="radios">
			{#each textCases as value}
				<label>
					<input type="radio" bind:group={textCase} name="case" {value} />
					{value}
				</label>
			{/each}
		</div>
	</div>

	<div class="result" class:empty={!result}>
		<p>{result ? result : `No input yet`}</p>
		<button on:click={copyResult}>📋</button>
	</div>

	<div class="notifications">
		<!-- {#if notification} -->
		<div class="content hidden" bind:this={notificationsRef}>
			<p>Copied!</p>
		</div>
		<!-- {/if} -->
	</div>
</div>

<style>
	.container {
		max-width: 400px;
		margin: 0 auto;
		/* margin-top: 5rem; */
	}

	.intro {
		margin-bottom: 4rem;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.intro p {
		text-align: center;
		margin: 0;
		font-size: 0.8rem;
	}
	.input {
		display: flex;
		/* gap: 10px; */
		align-items: baseline;
		border-bottom: 1px solid teal;
	}

	input {
		flex-grow: 1;
		/* max-width: 260px; */
		font-size: 1.4rem;
		outline: none;
		border: none;
		color: teal;
		background: white;
		font-weight: bold;
		padding: 0.4rem;
	}

	input::placeholder {
		font-size: 1rem;
		font-weight: normal;
	}

	.input button {
		background: transparent;
		border: none;
		cursor: pointer;
	}

	.options {
		padding: 0 2rem;
		margin-top: 2rem;
	}

	.options .radios {
		display: flex;
		justify-content: space-between;
		font-size: 0.8rem;
	}

	.radios label {
		display: flex;
		justify-content: center;
		/* align-items: center; */
		cursor: pointer;
	}

	.result {
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 2rem;
	}

	.result p {
		font-size: 1.5rem;
		font-weight: bold;
		margin: 0;
		word-break: break-all;
	}

	.result:hover button {
		display: inline-block;
	}

	.result button {
		margin-left: 10px;
		outline: none;
		border: none;
		cursor: pointer;
		box-shadow: 1px 2px 3px #eee;
		transition: all 0.1s;
		font-size: 1.5rem;
		display: none;
	}

	.result.empty p {
		font-size: 1rem;
		font-weight: normal;
	}

	.result.empty:hover button {
		display: none;
	}

	.notifications {
		display: flex;
		justify-content: center;
		align-items: center;
		transition: all 0.2s;
		user-select: none;
		margin-top: 2rem;
	}

	.notifications .content {
		width: 60px;
		height: 24px;
		background: teal;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 5px;
		font-size: 0.8rem;
		transition: all 0.2s ease-in-out;
	}

	.notifications p {
		color: white;
		font-weight: bold;
	}

	.content.hidden {
		visibility: hidden;
	}
</style>

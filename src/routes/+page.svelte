<script lang="ts">
	let history = [];
	let message = '';
	let answer = '';
	let loading = false;
	async function onSend() {
		console.log(message);
		loading = true;
		body: message;

		history = [...history, ['human', message]];
		message = '';
		const response = await fetch('/api/chat', {
			method: 'POST',
			body: JSON.stringify({ history })
		});
		const data = await response.json();
		history = [...history, ['assistant', data.message]];
		loading = false;
	}
</script>

<main class="flex flex-col items-center h-screen">
	{#each history as h}
		<div
			class="flex p-4 border-2 border-opacity-85 m-1 rounded-md text-white {h[0] == 'human'
				? 'bg-blue-500 flex justify-end'
				: 'bg-red-500 flex justify-start'}"
		>
			{h[1]}
		</div>
	{/each}
	<div class="align-bottom">
		<form on:submit={onSend}>
			<input bind:value={message} class="bg-orange-50 p-2 m-3" placeholder="Your message here..." />
			<button disabled={loading} type="submit">Send</button>
		</form>
	</div>
</main>

<script lang="ts">
	import { enhance } from '$app/forms';
	import { invalidate } from '$app/navigation';
	import { makeClient } from '$lib/make-client.js';

	export let data;
	const client = makeClient(fetch);

	let isLoading = false;
	let taskName = '';

	async function handleActionClick(id: string, action: `${keyof (typeof client.tasks)[':id']}`) {
		try {
			isLoading = true;
			await client.tasks[':id'][action].$post({
				param: { id }
			});
			await invalidate(client.tasks.$url());
		} catch (error) {
			console.error(error);
		} finally {
			isLoading = false;
		}
	}
</script>

<h1 class="text-center text-4xl font-bold">BTMW: The best task manager in the world</h1>

<div class="flex flex-col items-center justify-center">
	<h2 class="text-2xl font-bold">New Task</h2>
	<form method="POST" use:enhance class="flex gap-5">
		<input
			type="text"
			name="name"
			required
			bind:value={taskName}
			disabled={isLoading}
			autofocus={true}
			autocorrect="off"
			autocomplete="off"
			placeholder="New Task"
			class="border-lightgray rounded-lg border-2 p-2 text-2xl focus:border-blue-500 focus:outline-none focus:ring-4 focus:ring-blue-300"
		/>
		<button type="submit" disabled={isLoading} class="rounded-lg p-2 text-2xl"> Add </button>
	</form>
</div>

<div class="flex flex-col items-center justify-center">
	<h2 class="text-2xl font-bold">My Tasks</h2>
	{#if data.tasks.length === 0}
		<p class="text-2xl">You don't have any tasks! Be free little bird</p>
	{:else}
		<ul>
			{#each data.tasks as task (task.id)}
				<li class="flex flex-col items-center justify-center">
					<div class="flex justify-between">
						{task.done ? '☑️' : '⬛️'}
						<span class="text-2xl">{task.name}</span>
					</div>
					<div class="flex gap-2">
						{#if !task.done}
							<button
								type="button"
								on:click={() => handleActionClick(task.id, 'finish')}
								class="rounded-lg bg-green-500 p-2 text-2xl hover:bg-green-700"
							>
								Finish
							</button>
						{:else}
							<button
								type="button"
								on:click={() => handleActionClick(task.id, 'undo')}
								class="rounded-lg bg-blue-500 p-2 text-2xl hover:bg-blue-700"
							>
								Undo
							</button>
							<button
								type="button"
								on:click={() => handleActionClick(task.id, 'delete')}
								class="rounded-lg bg-red-500 p-2 text-2xl hover:bg-red-700"
							>
								Delete
							</button>
						{/if}
					</div>
				</li>
			{/each}
		</ul>
	{/if}
</div>

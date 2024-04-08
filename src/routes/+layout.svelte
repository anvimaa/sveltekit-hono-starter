<script>
	import '../app.pcss';
</script>

<!-- hidden scrollbar horizontal and vertical -->
<svelte:head>
	<title>Hone Server With Shadcn UI</title>
</svelte:head>

<header class="flex items-center justify-between bg-gray-500 px-8 py-4">
	<a href="/">
		<img src="/favicon.png" alt="logo" width="32" height="32" />
	</a>
	<nav>
		<a href="/">Home</a>
		<a href="/tasks">Tasks</a>
	</nav>
</header>

<div class="grid h-screen place-content-center">
	<slot />
</div>

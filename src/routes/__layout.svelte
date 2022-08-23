<script>
	import Header from '$lib/header/Header.svelte';
	import '../app.css';
</script>

<main>
	<slot />
</main>

<footer class="mt-3 bg-light">
	<div class="container">
		<div>
			<p class="small mb-0 text-center">題庫已更新為交通部航港局 1110728 發布版本</p>
		<div>
	</div> 
</footer>

<style>
	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 1024px;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 16px;
	}

	footer a {
		font-weight: bold;
	}

	@media (min-width: 480px) {
		footer {
			padding: 16px 0;
		}
	}
</style>

<script lang="ts">
	import { onMount } from 'svelte';
	import Calculator from './components/Calculator.svelte';
	export let name: string;

	onMount(() => {
		console.log('the app has mounted');
	});
</script>

<main>
	<h1>{name}!</h1>
	<div class="home">
		<Calculator/>
	</div>
	
</main>

<style>
	main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
        width: 100%;
        height: 100%;
		overflow: hidden;
	}
	.home {
        width: 100%;
        height: 100%;
		align-items: center;
        justify-content: start;
		display: flex;
		flex-direction: column;
		overflow: hidden;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
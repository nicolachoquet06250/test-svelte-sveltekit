<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		to your new<br />SvelteKit app
	</h1>

	<h2>
		try editing <strong>src/routes/+page.svelte</strong>
	</h2>

	<Counter />

	<MyCounter min={1} max={10} />

	<MyCounter min={1} max={10} loop={false} />

	<MyCounter />
</section>

<script>
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';
    import MyCounter from './MyCounter.svelte';
	import { setContext } from 'svelte';
	import { writable } from 'svelte/store';

	setContext('maxValue', 12);
	setContext('minValue', -10);

	const count = writable(0, () => {
		console.log('got a subscriber');
		return () => console.log('no more subscribers');
	});

	count.set(1); // does nothing

	const unsubscribe = count.subscribe(value => {
		console.log(value);
	}); // logs 'got a subscriber', then '1'

	unsubscribe(); // logs 'no more subscribers'
</script>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>

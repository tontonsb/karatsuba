<script>
	import {randomInteger} from './helpers'
	import Classic from './components/Classic.svelte'
	import Karatsuba from './components/Karatsuba.svelte'

	let x = randomInteger(1000000000000000, 10000000000000000)
	let y = randomInteger(1000000000000000, 10000000000000000)

	let cutoff = 10000000
</script>

<main>
	<h1>Karatsuba algorithm</h1>

	<p>In 1960 Andrei Kolmogorov said something along the lines of "To multiply two numbers you have to multiply each digit of one by each digit of the other. There's no faster way."</p>
	<p>"Why do you take that for granted?" thought Anatoly Karatsuba. And here's what he came up with.</p>

	<label>
		x = <input bind:value={x} >
	</label>
	<label>
		y = <input bind:value={y} >
	</label>
	<label>
		Fall back to classic for values below <input bind:value={cutoff} >
	</label>

	<div>
		<section>
			<h2>Classic</h2>
			<Classic x={x} y={y} />
		</section>

		<section>
			<h2>Karatsuba</h2>
			<Karatsuba x={x} y={y} {cutoff} />
		</section>
	</div>
</main>

<style>
	div {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 1rem;
	}

	section {
		background: #f8f8fc;
		padding: .5rem;
	}
</style>
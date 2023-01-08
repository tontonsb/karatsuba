<script>
	import KaratsubaPart from './KaratsubaPart.svelte'

	export let x
	export let y
	export let cutoff = 10000

	export let result = 0
</script>

<div>
	<KaratsubaPart {x} {y} {cutoff} bind:result />
</div>

<style>
div {
	counter-reset: karatsuba-line;
}
</style>

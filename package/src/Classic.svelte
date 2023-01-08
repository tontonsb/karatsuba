<script>
	import ClassicPart from './ClassicPart.svelte'

	export let x
	export let y
</script>

<div>
	<ClassicPart
		x={BigInt(x)}
		y={BigInt(y)}
	/>
</div>

<style>
div {
	counter-reset: karatsuba-line;
}
</style>

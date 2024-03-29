# Karatsuba algorithm visualization for Svelte

This provides a Svelte component to show Karatsuba's algorithm with all the
steps.

## Installation

```
npm install @tontonsb/karatsuba-display
```

## Usage

```svelte
<script>
import {Karatsuba} from '@tontonsb/karatsuba-display'

// Numbers to multiply
const x = 6561732515444972
const y = 3706488315741638

// Limit under which lower numbers will be multiplied using the classic algo
const cutoff = 10000000
</script>

<Karatsuba x={x} y={y} cutoff={cutoff}>
```

This package also exports a visualization of the classic digit-by-digit
multiplication algorithm, you can include both for comparison:

```svelte
<script>
import {Karatsuba, Classic} from '@tontonsb/karatsuba-display'

const x = 4125652941400349
const y = 8096998607037776

const cutoff = 10000000
</script>

<Karatsuba {x} {y} {cutoff} />
<Classic {x} {y} />
```

Integers, BigInts and strings are supported:

```svelte
<script>
import {Karatsuba} from '@tontonsb/karatsuba-display'

const x = '4125652941400349'
const y = 8096998607037776n

const cutoff = 10000000n
</script>

<Karatsuba {x} {y} {cutoff} />
```

## Styling

You may style the display by targeting the following selectors:

- `pre` for the whole display
- `pre:target` for the active (user-selected) sub-calculation
- `.comment` for calculation explanations on each row
- `.counter::before` for row numbers

Here is an example setup:

```svelte
<section>
<Karatsuba x={7111704934287416} y={9561121091923196} cutoff={10000000} />
</section>

<style>
section > :global(pre) {
	letter-spacing: .1em;
}

section > :global(pre:target) {
	border: 4px solid green;
}

section > :global(.counter::before) {
	border-right: 1px solid #ddd;
	padding: 0 .5em;
	margin-right: .5em;
	color: #888
}
</style>
```

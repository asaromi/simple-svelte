<script>
	import { page } from '$app/stores'
	import { spring } from 'svelte/motion'
	import { createEventDispatcher, onMount } from 'svelte'

    export let totalData = 0

	const displayed_currentPage = spring()
	const dispatch = createEventDispatcher()

	let currentPage
	let limit
	$: currentPage = $page.url.searchParams.get('page') || 1
	$: limit = $page.url.searchParams.get('limit') || 10


	$: displayed_currentPage.set(parseInt(currentPage > 0 ? currentPage : '1'))
	$: offset = modulo($displayed_currentPage, 1)
	$: end = currentPage * limit
	$: start = end - limit + 1

	onMount(() => {
		displayed_currentPage.set(parseInt(currentPage))
	})

	/**
	 * @param {number} n
	 * @param {number} m
	 */
	function modulo(n, m) {
		// handle negative numbers
		return ((n % m) + m) % m
	}

	function nextPage() {
		dispatch('next')
	}

	function previousPage() {
		dispatch('previous')
	}
</script>

<div>
    <div class="paginate">
        <button
            disabled={currentPage <= 1}
            aria-label="Decrease the paginate by one"
            style={currentPage <= 1 ? 'opacity: 0.5;pointer-events:none;' : ''}
            on:click={previousPage}
        >
            <svg aria-hidden="true" viewBox="0 0 1 1">
                <path d="M0,0.5 L1,0.5"/>
            </svg>
        </button>

        <div class="paginate-viewport">
            <div class="paginate-digits" style="transform: translate(0,{100 * offset}%)">
                <strong>{Math.floor($displayed_currentPage)}</strong>
            </div>
        </div>

        <button on:click={nextPage} aria-label="Increase the paginate by one">
            <svg aria-hidden="true" viewBox="0 0 1 1">
                <path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1"/>
            </svg>
        </button>
    </div>

    <div class="w-100 flex justify-center">
        <span>Result for data {totalData > 0 ? `${start} - ${end}` : totalData}</span>
    </div>
</div>

<style>
    .paginate {
        display: flex;
        border-top: 1px solid rgba(0, 0, 0, 0.1);
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        margin: 1rem 0;
    }

    .paginate button {
        width: 2em;
        padding: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        border: 0;
        background-color: transparent;
        touch-action: manipulation;
        font-size: 2rem;
    }

    .paginate button:hover {
        background-color: var(--color-bg-1);
    }

    svg {
        width: 25%;
        height: 25%;
    }

    path {
        vector-effect: non-scaling-stroke;
        stroke-width: 2px;
        stroke: #444;
    }

    .paginate-viewport {
        width: 4em;
        height: 2em;
        overflow: hidden;
        text-align: center;
        position: relative;
    }

    .paginate-viewport strong {
        position: absolute;
        display: flex;
        width: 100%;
        height: 100%;
        font-weight: 400;
        color: var(--color-theme-1);
        font-size: 1.5rem;
        align-items: center;
        justify-content: center;
    }

    .paginate-digits {
        position: absolute;
        width: 100%;
        height: 100%;
    }
</style>

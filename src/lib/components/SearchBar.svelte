<script>
	import { createEventDispatcher, onMount } from 'svelte'

    const dispatch = createEventDispatcher()
    let query
    let value = ''

    onMount(() => {
        query = new URLSearchParams(window.location.search)
        value = query.get('q')
    })

    const handleChange = (e) => {
		e.preventDefault()
		dispatch('change', e.target.value)
    }
</script>

<label class="w-full flex align-items-center mb-4 gap-x-2.5">
    <span>Search:</span>
    <input
        class="w-full"
        type="search"
        placeholder="Enter to search..."
        {value}
        on:change={handleChange}
    />
</label>

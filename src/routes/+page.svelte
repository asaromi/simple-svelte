<script>
	import { goto } from '$app/navigation'
	import { onMount } from 'svelte'

	import Pagination from '$lib/components/Pagination.svelte'
	import CardList from '$lib/components/CardList.svelte'
	import Loader from '$lib/components/Loader.svelte'
	import SearchBar from '$lib/components/SearchBar.svelte'

	let query
	let currentPage = 1
	let limit = 10
	let searchQuery = ''
	let promisePhotos
    let totalData = 0

	onMount(() => {
		query = new URLSearchParams(location.search)
		currentPage = query.get('page') && parseInt(query.get('page')) || 1
		limit = query.get('limit') && parseInt(query.get('limit')) || 10
		searchQuery = query.get('q') || ''

		promisePhotos = fetchData()
	})

	async function fetchData() {
		try {
			let queryParams = new URLSearchParams(location.search)
			if (currentPage >= 1) {
				queryParams.set('_page', currentPage)
			}
			if (limit >= 1) {
				queryParams.set('_limit', limit)
			}
			if (searchQuery) {
				queryParams.set('q', searchQuery)
			} else {
				queryParams.delete('q')
			}

			const res = await fetch(`https://jsonplaceholder.typicode.com/photos?${queryParams.toString()}`)
			if (!res.ok) throw new Error('failed to fetch data')

            const data = await res.json()
            totalData = data.length
			return data
		} catch (error) {
			alert(error.message)
			throw error
		}
	}

	async function nextPage() {
		currentPage++
		await query.set('page', currentPage)
		goto(`?${query.toString()}`)
		promisePhotos = fetchData()
	}

	async function prevPage() {
		currentPage--
		await query.set('page', currentPage)
		goto(`?${query.toString()}`)
		promisePhotos = fetchData()
	}

	async function searchChange(e) {
		const { detail: value } = e

		searchQuery = value || ''
		await query.set('q', searchQuery)
		goto(`?${query.toString()}`)
		promisePhotos = fetchData()
	}
</script>

<svelte:head>
    <title>Home</title>
    <meta name="description" content="Svelte demo app"/>
</svelte:head>

<section class="w-full min-w-1/3">
    <h1 class="font-semibold">Fetching data</h1>
    <div class="w-1/2">
        <SearchBar on:change={searchChange}/>
    </div>

    {#await promisePhotos}
        <Loader size="w-12 h-12"/>
    {:then photos}
        <CardList data={photos}/>
    {/await}

    <Pagination {totalData} on:next={nextPage} on:previous={prevPage}/>
</section>

<style>
    section {
        display: flex;
        flex-direction: column;
        justify-content: start;
        align-items: center;
        flex: 0.6;
    }

    h1 {
        width: 100%;
    }
</style>

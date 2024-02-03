<script>
    import Card from '$lib/components/Card.svelte'

    export let data =[]
</script>

<div class="w-fit block space-y-4">
    {#if data.length === 0}
        <p class="p-4">No data found</p>
    {/if}

    {#each data as item}
        <Card {...item} />
    {/each}
</div>

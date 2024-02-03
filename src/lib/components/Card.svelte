<script>
	export let id
    export let albumId
	export let thumbnailUrl
	export let title
	export let url
</script>

<div class="w-full flex border-opacity-50 p-4 rounded-8 space-x-4">
    <a class="w-fit p-0 border-0" href={thumbnailUrl} target="_blank" rel="noreferrer">
        <img class="w-fit h-full aspect-square" src={thumbnailUrl} alt={title}/>
    </a>

    <div class="w-full relative">
        <p class="leading-none">id: {id}</p>
        <p class="leading-none">album id: {albumId}</p>
        <p class="leading-none">title: {title}</p>

        <a class="absolute bottom-0 right-0 btn" href={url} target="_blank" rel="noreferrer">Visit here</a>
    </div>
</div>

<script>
let apiKey = 'ysqpmjZJTL9ncfMAHBgNfciO1V3sjkxh'
let q = ''
const limit = 1
let giphy
import { fade, fly, scale } from 'svelte/transition'
import { HeartIcon} from 'svelte-eva-icons'

const getImage = () => {
	giphy = null
fetch(`https://api.giphy.com/v1/gifs/search?q=${q}&limit=${limit}&apiKey=${apiKey}`)
	.then( res => res.json() )
		.then( json => {
			console.log(json)
			giphy = json.data[0].images.downsized_medium.url
		})
}

let favorites = []

const addToFav = (element) => {
	if (!favorites.includes(element)) {
		favorites = [element, ...favorites]
	} else {
		favorites = favorites.filter( e => e != element)
	}
}

let showFav = false
</script>

<main>
	{#if !showFav}
		{#if giphy}
			<img in:scale src="{giphy}" alt="{q}" class="giphy">
			<div class="heart" 
			on:click={ () => addToFav(giphy)}
			style={favorites.includes(giphy) ? 'fill:red' : 'fill:gray'}>
			<HeartIcon/>
			</div>
		{:else}
			<h2>Fetching GIF...</h2>
		{/if}
	{:else}
		<div in:scale class="favorites">
			{#each favorites as fav}
				<img src="{fav}" alt="gif">
			{/each}
		</div>
	{/if}

	<header>
		<input bind:value={q} 
		placeholder="Type to search" 
		on:keydown={(key)=>key.key == "Enter"? getImage():""}
		on:click={ key => key.target.value = ''}
		on:focus={ key => key.target.value = ''}
		>

		<button on:click={getImage}>Get GIF</button>

		{#if favorites.length > 0}
		<button in:scale on:click={ () => showFav = !showFav }>
		{ showFav ? 'Hide favorites' : 'Show favorites'}
		</button>
		{/if}

	</header>

</main>

<style>
	:global(body, html){
		margin: 0;
		padding: 0;
	}
	:global(*){
		box-sizing: border-box;
	}
	main{
		display: grid;
		place-items: center;
		height: 100%;
		position: relative;
	}
	header{
		position: absolute;
		top: 2rem;
		width: 100%;
		display: grid;
		padding: 0 20vw 0 20vw;
	}
	.giphy{
		width: 40vw;
		height: 40vh;
		background-color: white;
		padding: 2rem;
	}
	input{
		outline: none;
	}
	.heart{
		position: absolute;
		bottom: 3rem;
		height: 5rem;
		width: 5rem;
		fill: gray;
		transition: .4s ease;
	}
	img{
		max-height: 40vh;
		width: 60vw;
		object-fit: cover;
	}
	.favorites{
		max-height: 60vh;
		overflow: scroll;
		display: grid;
		gap: 2rem;
		grid-template-columns: repeat(4, 200px);
	}
	.favorites img{
		width: 100%;
		height: 200px;
		object-fit: cover;
	}
</style>
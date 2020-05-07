<script>
let apiKey = 'ysqpmjZJTL9ncfMAHBgNfciO1V3sjkxh'
let q = ''
const limit = 1
let giphy

const getImage = () => {
fetch(`https://api.giphy.com/v1/gifs/search?q=${q}&limit=${limit}&apiKey=${apiKey}`)
	.then( res => res.json() )
		.then( json => {
			console.log(json)
			giphy = json.data[0].images.downsized_medium.url
		})
}
</script>

<main>
	{#if giphy}
		<img src="{giphy}" alt="{q}" class="giphy">
	{:else}
		<h2>Fetching GIF</h2>
	{/if}

	<header>
		<input bind:value={q} placeholder="pog" on:keydown={(key)=>key.key == "Enter"? getImage():""}>
		<button on:click={getImage}>Get GIF</button>
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
</style>
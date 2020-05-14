<script>

let stock_id
let card

//get card stock
fetch('https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1')
	.then(res=>res.json())
	.then(json => stock_id = json.deck_id)

//draw a card
const getCard = () => { 
	fetch(`https://deckofcardsapi.com/api/deck/${stock_id}/draw/?count=1`)
	.then(res=>res.json())
	.then(json=>
		{
			console.log(json)
			card = json.cards[0]
		}
	)
}

let response = ''

$: if(card){
	if(card.code.includes('A')){
		response = 'Waterfall!'
	}
	if(card.code.includes('2')){
		response = 'Velg en som må drikke!'
	}
	if(card.code.includes('3')){
		response = 'Du må drikke!'
	}
	if(card.code.includes('4')){
		response = 'Alle jenter drikker!'
	}
	if(card.code.includes('5')){
		response = 'Tommelsjef! Når du legger tommelen på bordet, må alle følge og sistemann må drikke. Du er tommelsjef til noen andre trekker en femmer.'
	}
	if(card.code.includes('6')){
		response = 'Alle gutter drikker!'
	}
	if(card.code.includes('7')){
		response = 'Heaven. Når du peker fingeren opp i været, må alle følge og sistemann må drikke. Dette varer til det er din tur igjen.'
	}
	if(card.code.includes('8')){
		response = 'Velg en drikkepartner! Partnerskapet varer ut spillet.'
	}
	if(card.code.includes('9')){
		response = 'Rim! Velg et ord og personen ved siden av deg må rime, slik går det i en sirkel, til rotekoppen må drikke.'
	}
	if(card.code.includes('0')){
		response = 'Kategori! Velg en kategori som fotball, så går du i en sirkel, og alle må si et ord som passer med fotball som: mål, straffe, tippeligaen. Rotekoppen drikker.'
	}
	if(card.code.includes('J')){
		response = 'Regel! Du kan lage hvilken som helst regel som alle må følge, for eksempel at du bare kan drikke med venstre hånd. Alle (inkludert deg) må følge denne regelen resten av spillet, glemmer du deg må du drikke.'
	}
	if(card.code.includes('Q')){
		response = 'Spørsmål! Hvis noen svarer på et spørsmål må de drikke, dette varer ut runden.'
	}
	if(card.code.includes('K')){
		response = 'Sharing is caring! Alle må helle litt av drikken sin i en kopp, sistemann som trekker Konge må drikke hele koppen.'
	}
}



//sett op en knapp som ved klik henter et kort og viser det på skærmen
//ha en funksjone som evaluerer hva som skal skje 

</script>

<main>
	<div class="card">
	{#if card}
	<div>
		<p class="response">{response}</p>	
		<img src={card.image} alt={card.code}>
	</div>
	{:else}
		<p>Trekk et kort for å spille Ring of Fire!</p>
	{/if}
	</div>

	<button on:click={getCard}>Trekk kort</button>
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
		height: 100vh;
		position: relative;
	}
	button{
		position: absolute;
		bottom: 7rem;
		cursor: pointer;
		border-radius: 5px;
	}
	.card{
		text-align: center;
		bottom: 10rem;
	}
	.response{
		padding-bottom: 5rem;
		width: 40vw;
	}
</style>
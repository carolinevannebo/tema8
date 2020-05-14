<script>
	const { remote } = require('electron')
	const { Menu, MenuItem } = remote
	let info = 'Nothing happening yet'

	const showNotification = () => {
		let myNotification = new Notification('Hello', {
			body: 'You are now officially part of the system OS'
		})
		myNotification.onclick = () => {
			info = 'Notification clicked'
		}
	}

	const amIOnline = () => {
		window.alert(navigator.onLine ? 'You\'re online ma\'am' : 'you\'re offline' )
		info = 'Alert accepted'
	}

	const menu = new Menu()
	menu.append(new MenuItem({ label: 'meny 1', click() { info = 'item 1 clicked' } }))
	menu.append(new MenuItem({ type: 'separator' }))
	menu.append(new MenuItem({ label: 'meny 2', click() { info = 'item 2 clicked' } }))

	const context = e => {
		e.preventDefault()
		menu.popup({ window: remote.getCurrentWindow() })
	}
</script>

<main>
	<div class="stuff" on:contextmenu={context}></div>
	<h1>Svelte in Electron</h1>
	<p>{info}</p>
	<button on:click = { () => showNotification() }>Click me</button>
	<button on:click = { () => amIOnline() }>Online?</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
	.stuff {
		width: 100%;
		height: 50px;
		background-color: slategray;
	}
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
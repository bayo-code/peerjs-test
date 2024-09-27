<script lang="ts">
	import { Peer } from "peerjs"
	import { onMount } from 'svelte';

	let peer = new Peer("waiter")

	let connected = false

	onMount(() => {
		let getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
		peer.on('call', (call) => {
			getUserMedia({audio: true}, (stream) => {
				call.answer(stream);
				call.on('stream', () => {
					connected = true;
				})
			})
		})
	})

</script>


<h1>{!connected ? 'Waiting for connection...' : 'Connected'}</h1>
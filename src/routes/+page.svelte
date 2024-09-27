<script lang="ts">
	import { Peer } from "peerjs"
	import { onMount } from 'svelte';

	let peer = new Peer()

	let connected = false
	let peerID = peer.id

	onMount(() => {
		let getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
		peer.on('call', (call) => {
			getUserMedia({audio: true}, (stream) => {
				call.answer(stream);
				call.on('stream', (remoteStream) => {
					connected = true;
					document.querySelector('#remote-audio').srcObject = remoteStream
				})
			})
		})
	})

</script>

<h1>Peer ID: {peerID}</h1>
<h1>{!connected ? 'Waiting for connection...' : 'Connected'}</h1>
<audio id="remote-audio" controls autoplay></audio>
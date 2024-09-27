<script lang="ts">
	import { Peer } from "peerjs"
	import { onMount } from 'svelte';

	let peer = new Peer()

	let connected = false
	let peerID = ''

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

		peer.on('open', function(id) {
			console.log('My peer ID is: ' + id);
			peerID = id;
		});
	})

</script>

<h1>Peer ID: {peerID}</h1>
<h1>{!connected ? 'Waiting for connection...' : 'Connected'}</h1>
<audio id="remote-audio" controls autoplay></audio>
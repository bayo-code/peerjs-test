<script lang="ts">
	import { Peer } from "peerjs"
	import { onMount } from 'svelte';

	let peer = new Peer("other")

	let connected = false

	onMount(() => {
		let getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
		getUserMedia({audio: true}, (stream) => {
			let call = peer.call('waiter', stream)
			console.log('Connected! Call: ', call);
			call.on('stream', (remoteStream) => {
				connected = true;
				document.querySelector('#remote-audio').srcObject = remoteStream
			})
		})
	})
</script>

<h1>{connected ? 'Connected' : 'Connecting'}</h1>
<audio id="remote-audio" controls autoplay></audio>
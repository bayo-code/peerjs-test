<script lang="ts">
	import { Peer } from "peerjs"

	let peer = new Peer("other")

	let connected = false
	let value: string = ""

	function connectToUser() {
		let getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
		getUserMedia({audio: true}, (stream) => {
			let call = peer.call(value, stream)
			console.log('Connected! Call: ', call);
			call.on('stream', (remoteStream) => {
				connected = true;
				document.querySelector('#remote-audio').srcObject = remoteStream
			})
		})
	}
</script>

<input type="text" bind:value />
<button on:click={connectToUser}>
	Connect
</button>
<h1>{connected ? 'Connected' : 'Connecting'}</h1>
<audio id="remote-audio" controls autoplay></audio>
<script>
	import CoinFlip from '../components/tools/CoinFlip.svelte';
	import AnswerChooser from '../components/tools/AnswerChooser.svelte';
	import Clipboard from '../components/helper/Clipboard.svelte';
	import Alert from '../components/helper/Alert.svelte';

	let copyText;
	let showAlert;
</script>

<svelte:head>
	<title>Skayo's Funbox</title>
</svelte:head>

<div class="uk-grid-medium" uk-grid uk-sortable="handle: .uk-sortable-handle">
	<AnswerChooser {copyText} />
	<CoinFlip />
</div>

<Clipboard bind:copyText />
<Alert bind:showAlert />
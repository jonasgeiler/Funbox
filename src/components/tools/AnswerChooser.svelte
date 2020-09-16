<script>
	import WheelOfFortune from '../misc/WheelOfFortune.svelte';
	import Tool from './Tool.svelte';

	const answers = {
		'Deine Mama':                                    'LittleFreakBe',
		'Schöne Schuhe hast du':                         'dingusbread44',
		'normal ist das nicht':                          'Klirrkopf',
		'warum ist meine TL voll davon':                 'underthepancake',
		'Das muss ich mir jetzt echt nicht geben':       'dingusbread44',
		'Willst du mich eigentlich komplett verarschen': 'dingusbread44',
		'kmdd.de':                                       'dingusbread44',
		'Pisse aus meinem Arsch':                        'Klirrkopf',
		'Das ist mir jetzt bisschen unangenehm':         'dingusbread44',
		'Lustige Katzen 2020 #24':                       'fischtisch',
		'schon okay.':                                   'dingusbread44',
		'Oh Junge':                                      'dingusbread44',
		'eigentlich heiße ich Peter Müller':             'dingusbread44',
		'das gabs mal im Happy meal':                    'fischtisch',
		'ich habe gepupst':                              'MeInAnUsBrEnNt_',
		'achso, nein':                                   'fischtisch',
		'was erlaubst du dir eigentlich':                'fischtisch',
		'wir vermuten es schon seit Jahren':             'fischtisch',
		'Bist du nicht etwas zu alt dafür':              'OrNotTobey',
		'das ist ein zeichen patrick':                   'fischtisch',
		'nimmst du mich überhaupt ernst':                'fischtisch',
		'hä':                                            'fischtisch',
		'Ich bin ein großer Fan von dir':                'dingusbread44',
		'Ich werde bald schwanger':                      'dingusbread44',
		'achso ich dachte die drehen ein tik tok':       'fischtisch',
		'hättest du airpods wär dir das nicht passiert': 'fischtisch',
		'jetzt hab ich hunger':                          'fischtisch',
		'wenn du verstehst was ich meine':               'fischtisch',
	};

	let complete = false;
	let resultField;
	let result = 'wenn du verstehst was ich meine';

	export let copyText;

	function handleSpinComplete(_result) {
		result = _result;
		complete = true;
	}

	function select(element) {
		element.select();
		element.setSelectionRange(0, element.value.length);
	}
</script>

<Tool title="Answer Chooser (German)" size="large">
	<WheelOfFortune prizes={Object.keys(answers)} onComplete={handleSpinComplete} />

	{#if complete}
		<div class="uk-width-1-1 uk-flex uk-flex-center uk-margin-bottom">
			<span class="uk-width-auto uk-hiddden@s" data-uk-icon="icon: arrow-down; ratio: 4" data-uk-tooltip="pos: left" title="Result"></span>
		</div>

		<div class="uk-text-center">
			<input bind:this={resultField} on:click={select(resultField)} class="uk-input uk-form-large" type="text" readonly value={result}>
			<a class="uk-text-meta uk-margin-small-top" href="https://twitter.com/{answers[result]}">by @{answers[result]}</a>
		</div>

		<button on:click={copyText(result)} class="uk-button uk-button-primary uk-width-1-1 uk-margin-small-top">Copy</button>
	{/if}
</Tool>
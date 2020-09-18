<script context="module">
	const SLIDE_LIST_LENGTH = 5;
	const ANSWERS = {
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
		'Musst du nicht lernen?':                        'regenleben',
		'Gibt es das auch in besserer Qualität':         'OrNotTobey',
		'Moment, ich muss erst Skayo\'s Funbox fragen':  'OrNotTobey',
		'Das wäre dir mit Erdnuss nicht passiert':       'dingusbread44',
		'Ich blocke Sie jetzt!':                         '1kescher',
		'hab gehört das is recht ungesund':              'Skayo',
		'Das auch besser so, du':                        'dingusbread44',
		'da hilft nur noch onlyfans':                    '1kescher',
	};
</script>

<script>
	import { tick } from 'svelte';
	import Tool from './Tool.svelte';

	let slotItems = ['Click here to spin!'];

	let slotMachineContainer;
	let firstSpin = true;
	let result = '';

	let resultField;
	export let copyText;

	function makeSlotItems() {
		slotItems = [];

		for (let i = 0; i <= SLIDE_LIST_LENGTH; i++) {
			slotItems = [...slotItems, ...Object.keys(ANSWERS)];
		}
	}

	function randomAnswerElement() {
		const answerElements = slotMachineContainer.children;

		const randomIndex = Math.floor(Math.random() * answerElements.length);

		return answerElements[randomIndex];
	}

	async function handleClick() {
		if (firstSpin) {
			firstSpin = false;

			makeSlotItems();
		}

		result = '';

		await tick();

		// Play animation
		const answerElementToScrollTo = randomAnswerElement();
		slotMachineContainer.style.top = (-answerElementToScrollTo.offsetTop) + 'px';

		setTimeout(() => {
			result = answerElementToScrollTo.innerText.trim();
		}, 1100);
	}

	function select(element) {
		element.select();
		element.setSelectionRange(0, element.value.length);
	}
</script>

<Tool title="Answer Chooser (German)" size="large">
	<div on:click={handleClick} class="slot-machine">
		<div bind:this={slotMachineContainer} class="slot-machine-container">
			{#each slotItems as slotItem}
				<div class="slot-machine-item">
					<h1 class="slot-machine-item-title">{slotItem}</h1>
				</div>
			{/each}
		</div>
	</div>

	{#if !firstSpin && result !== ''}
		<div class="uk-width-1-1 uk-flex uk-flex-center uk-margin-bottom">
			<span class="uk-width-auto" data-uk-icon="icon: arrow-down; ratio: 3" data-uk-tooltip="pos: left" title="Result"></span>
		</div>

		<div class="uk-text-center">
			<input bind:this={resultField} on:click={select(resultField)} class="uk-input uk-form-large" type="text" readonly value={result}>
			<a class="uk-text-meta uk-margin-small-top" href="https://twitter.com/{ANSWERS[result]}">by @{ANSWERS[result]}</a>
		</div>

		<button on:click={copyText(result)} class="uk-button uk-button-primary uk-width-1-1 uk-margin-small-top">Copy</button>
	{/if}
</Tool>

<style lang="scss">
	$slot-machine-item-height: 80px;

	.slot-machine {
		position: relative;
		width: 100%;
		overflow: hidden;
		height: $slot-machine-item-height;
		cursor: pointer;

		&-container {
			transition: top ease-in-out 1s;
			position: absolute;
			width: 100%;
			top: 0;
		}

		&-item {
			height: $slot-machine-item-height;
			display: flex;
			justify-content: center;
			align-items: center;
			white-space: nowrap;

			&-title {
				margin: 0 !important;
				font-weight: bold;
				overflow: hidden;
				text-overflow: ellipsis;
				line-height: $slot-machine-item-height;
			}
		}
	}
</style>
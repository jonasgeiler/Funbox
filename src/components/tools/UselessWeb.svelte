<script context="module">
	const USELESS_SITES = [
		"http://heeeeeeeey.com/",
		"http://corndog.io/",
		"https://alwaysjudgeabookbyitscover.com",
		"http://thatsthefinger.com/",
		"http://cant-not-tweet-this.com/",
		"http://weirdorconfusing.com/",
		"http://eelslap.com/",
		"http://www.staggeringbeauty.com/",
		"http://burymewithmymoney.com/",
		"https://smashthewalls.com/",
		"https://jacksonpollock.org/",
		"http://endless.horse/",
		"http://www.trypap.com/",
		"http://www.republiquedesmangues.fr/",
		"http://www.movenowthinklater.com/",
		"http://www.partridgegetslucky.com/",
		"http://www.rrrgggbbb.com/",
		"http://beesbeesbees.com/",
		"http://www.koalastothemax.com/",
		"http://www.everydayim.com/",
		"http://randomcolour.com/",
		"http://cat-bounce.com/",
		"http://chrismckenzie.com/",
		"https://thezen.zone/",
		"http://hasthelargehadroncolliderdestroyedtheworldyet.com/",
		"http://ninjaflex.com/",
		"http://ihasabucket.com/",
		"http://corndogoncorndog.com/",
		"http://www.hackertyper.com/",
		"https://pointerpointer.com",
		"http://imaninja.com/",
		"http://drawing.garden/",
		"http://www.ismycomputeron.com/",
		"http://www.nullingthevoid.com/",
		"http://www.muchbetterthanthis.com/",
		"http://www.yesnoif.com/",
		"http://lacquerlacquer.com",
		"http://potatoortomato.com/",
		"http://iamawesome.com/",
		"https://strobe.cool/",
		"http://www.pleaselike.com/",
		"http://crouton.net/",
		"http://corgiorgy.com/",
		"http://www.wutdafuk.com/",
		"http://unicodesnowmanforyou.com/",
		"http://chillestmonkey.com/",
		"http://scroll-o-meter.club/",
		"http://www.crossdivisions.com/",
		"http://tencents.info/",
		"http://www.patience-is-a-virtue.org/",
		"http://pixelsfighting.com/",
		"http://isitwhite.com/",
		"https://existentialcrisis.com/",
		"http://onemillionlols.com/",
		"http://www.omfgdogs.com/",
		"http://oct82.com/",
		"http://chihuahuaspin.com/",
		"http://www.blankwindows.com/",
		"http://dogs.are.the.most.moe/",
		"http://tunnelsnakes.com/",
		"http://www.trashloop.com/",
		"http://www.ascii-middle-finger.com/",
		"http://spaceis.cool/",
		"http://www.donothingfor2minutes.com/",
		"http://buildshruggie.com/",
		"http://buzzybuzz.biz/",
		"http://yeahlemons.com/",
		"http://wowenwilsonquiz.com",
		"https://thepigeon.org/",
		"http://notdayoftheweek.com/",
		"http://www.amialright.com/",
		"http://nooooooooooooooo.com/",
		"https://greatbignothing.com/",
		"https://zoomquilt.org/",
		"https://dadlaughbutton.com/",
		"https://www.bouncingdvdlogo.com/",
		"https://doggos.kescher.at/",
	];

	const RANDOM_RANGE = 7;
</script>

<script>
	import { onMount } from 'svelte';
	import Tool from './Tool.svelte';

	let initialClick = false;
	let sites = [];

	function selectWebsite () {
		const range = RANDOM_RANGE > sites.length ? sites.length : RANDOM_RANGE;
		const index = Math.floor(Math.random() * range);

		const site = sites[index];
		sites.splice(index, 1);

		return site;
	}

	function openSite(url) {
		window.open(url);
	}

	function handleClick() {
		initialClick = true;

		const url = selectWebsite();
		openSite(url);

		// User has visited ALL the sites... refresh the list.
		if (sites.length === 0) {
			sites = USELESS_SITES.slice();
		}

		localStorage['currentSiteList'] = JSON.stringify(sites);
	}

	onMount(() => {
		// Initial set sites
		sites = USELESS_SITES.slice();

		if (localStorage['currentSiteList']) {
			// They have storage, filter out any not in the base list, that could be spam now
			const currentSites = JSON.parse(localStorage['currentSiteList']);

			const filteredSites = currentSites.filter(
				(site) => USELESS_SITES.indexOf(site) !== -1,
			);

			if (filteredSites.length > 0) {
				sites = filteredSites;
			}
		}
	});
</script>

<Tool title="Useless Web" size="small">
	<div class="uk-text-center">
		<h1 class="uk-margin-remove sk-monospace">TAKE ME</h1>
		<h2 class="uk-margin-remove sk-monospace">TO {initialClick ? 'ANOTHER' : 'A'}</h2>
		<h1 class="uk-margin-remove sk-monospace">USELESS</h1>
		<h1 class="uk-margin-remove sk-monospace">WEBSITE</h1>

		<button on:click={handleClick} class="uk-margin-top uk-margin-small-bottom sk-monospace">PLEASE</button>
	</div>
</Tool>

<style lang="scss">
	h1 {
		text-shadow: 2px 2px 4px #999;
		line-height: 68px;
		font-size: 68px;
	}

	h2 {
		text-shadow: 2px 2px 4px #999;
		line-height: 30px;
		font-size: 30px;
	}

	button {
		padding: 2px 10px;
		background-color: #ff1493;
		color: #fff;
		text-shadow: none;
		box-shadow: 1px 0 1px #be3077, 0 1px 1px #ff69b4, 2px 1px 1px #be3077, 1px 2px 1px #ff69b4, 3px 2px 1px #be3077, 2px 3px 1px #ff69b4, 4px 3px 1px #be3077, 3px 4px 1px #ff69b4, 5px 4px 1px #be3077, 4px 5px 1px #ff69b4, 6px 5px 1px #be3077;
		cursor: pointer;
		display: inline-block;
		position: relative;
		box-sizing: border-box;
		user-select: none;
		border: 0;
		font-size: 45px;
		font-weight: 300;
		outline: 0;
		line-height: normal;
	}

	button:hover {
		background-color: #e21a62
	}

	button:active {
		box-shadow: 1px 0 1px #be3077, 0 1px 1px #ff69b4, 2px 1px 1px #be3077, 1px 2px 1px #ff69b4, 3px 2px 1px #be3077;
		top: 2px;
		left: 3px
	}
</style>

<script>
	import Event from './Event.svelte';

	function daysIntoYear(date) {
		return (Date.UTC(date.getFullYear(), date.getMonth(), date.getDate()) - Date.UTC(date.getFullYear(), 0, 0)) / 24 / 60 / 60 / 1000;
	}

	const width = 200;

	const events = {
		"1607": {
			"14 May": "Jamestown founded"
		},
		"1619": {
			"30 Jul": "House of Burgesses established"
		},
		"1620": {
			"18 Dec": "Plymouth Colony established"
		},
		"1629": {
			"4 Mar": "Massachusetts Bay Colony founded"
		},
		"1636": {
			"": "Rhode Island Colony founded"
		},
		"1637": {
			"7 Nov": "Anne Hutchinson banished from Massachusetts"
		},
		"1676": {
			"30 Jul": "Start of Bacon's rebellion",
			"19 Sep": "Jamestown burned by Bacon's forces",
			"26 Oct": "Nathaniel Bacon dies of dysentery: End of Bacon's rebellion"
		},
		"1692": {
			"2 Jun": "Salem Witch Trials"
		},
		"1733": {
			"17 May": "Molasses Act passed",
			"24 Jun": "Molasses Act put into enforcement"
		},
		"1754": {
			"5 Apr": "Start of the French and Indian War"
		},
		"1763": {
			"10 Feb": "Treaty of Paris ends the French and Indian War"
		},
		"1765": {
			"22 Mar": "Stamp Act passed",
			"1 Nov": "Stamp Act put into enforcement"
		},
		"1776": {
			"10 Jan": "Publication of Common Sense by Thomas Paine",
			"4 Jul": "Final version of the Declaration of Independence signed by Congress"
		},
		"1852": {
			"20 Mar": "Uncle Tom's Cabin published"
		},
		"1855": {
			"21 Nov": "Start of Bleeding Kansas"
		},
		"1857": {
			"6 Mar": "Dred Scott v. Sandford decision"
		},
		"1859": {
			"16 Oct": "John Brown's raid on Harpers Ferry"
		},
		"1860": {
			"6 Nov": "Abraham Lincoln elected as President"
		},
		"1861": {
			"8 Feb": "Creation of the Confederate States"
		},
		"1862": {
			"22 Sep": "Emancipation Proclamation signed"
		},
		"1973": {
			"17 May": "Congressional hearings on Watergate Scandal open"
		},
		"1974": {
			"9 Aug": "President Nixon resigns"
		},
		"1976": {
			"2 Nov": "Jimmy Carter elected as President"
		},
		"1977": {
			"7 Sep": "Torrijos–Carter Treaties signed"
		},
		"1992": {
			"3 Nov": "Bill Clinton elected as President"
		},
		"1996": {
			"5 Nov": "Bill Clinton re-elected for second term as President"
		},
		"2000": {
			"7 Nov": "George W. Bush elected as President"
		},
		"2004": {
			"2 Nov": "George W. Bush re-elected for second term as President"
		},
		"2008": {
			"4 Nov": "Barrack Obama elected as President"
		},
		"2012": {
			"6 Nov": "Barrack Obama re-elected for second term as President"
		}
	};

	function yearDateToNum(y, d) {
		const date = new Date(Date.parse(y + " " + d));
		return Number.parseInt(y) + (daysIntoYear(date) / 366);
	}

	const years = Object.keys(events);
	console.log(years)

	const fullEvents = {};
	years.forEach(a => {
		const year = a;
		const yearEvents = events[a];
		const dates = Object.keys(yearEvents);
		return dates.forEach(d => {
			fullEvents[yearDateToNum(year, d)] = {description: yearEvents[d], exactDate: d + " " + year};
		});
	});

	const fullYears = Object.keys(fullEvents);

	const firstYear = Math.min(...fullYears.map(Number.parseFloat));
	const lastYear = Math.max(...fullYears.map(Number.parseFloat));

	const fBefore = Math.floor(firstYear);
	const fAfter = Math.ceil(lastYear);

	for(var i = fBefore; i < fAfter; i++) {
		fullEvents[i] = fullEvents[i] || {description: "", exactDate: i.toString()}
	}

	const fullEntries = Object.entries(fullEvents);

	const smallestGap = Math.min(...fullYears.slice(0, fullYears.length - 1).map((a, i) => fullYears[i + 1] - a));

	const xPerYear = (width / smallestGap) * 1.2;

	const bottomW = (xPerYear * (lastYear - firstYear)) + width;

	function scrollHorizontally(e) {
		e = window.event || e;
		let delta = Math.max(-1, Math.min(1, (e.wheelDelta || -e.detail)));
		document.getElementsByTagName('html')[0].scrollLeft -= (delta*40); // Multiplied by 40
		e.preventDefault();
	}
</script>

<style>
	.timeline {
		border-bottom: 1px solid black;
		height: calc(100vh - 4em);
	}
</style>

<main id="main">
	<div id="timeline" on:mousewheel={scrollHorizontally} class="timeline" style="width: {bottomW.toFixed()}px;">
		{#each fullEntries as [fullYear, p]}
			<Event x="{(xPerYear * (fullYear - firstYear) + 24).toFixed()}" w="{width}" year="{p.exactDate}">
				{p.description}
			</Event>
		{/each}
	</div>
</main>

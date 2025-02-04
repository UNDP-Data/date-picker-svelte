<script lang="ts">
	import DatePicker from '$lib/DatePicker.svelte'
	import { localeFromDateFnsLocale } from '$lib/locale.js'
	import Prop from './prop.svelte'
	import Split from './split.svelte'
	import { hy, de, nb } from 'date-fns/locale'

	let value: Date | undefined = $state()
	let min: Date | undefined = $state()
	let max: Date | undefined = $state()
	let locales = [
		{ key: 'default', value: localeFromDateFnsLocale({}) },
		{ key: 'nb (date-fns)', value: localeFromDateFnsLocale(nb) },
		{ key: 'de (date-fns)', value: localeFromDateFnsLocale(de) },
		{ key: 'hy (date-fns)', value: localeFromDateFnsLocale(hy) },
	]
	let locale = $state(locales[3])
	let browseWithoutSelecting: boolean = $state(false)
	let timePrecision: 'minute' | 'second' | 'millisecond' | null = $state('millisecond')

	let disabledDay1 = new Date()
	let disabledDay2 = new Date(disabledDay1)
	disabledDay2.setDate(disabledDay1.getDate() + 1)
	let disabledDates = $state([disabledDay1, disabledDay2].filter((d) => d !== undefined))

	let enabledDates = $state([new Date()])
	const day2 = new Date()
	day2.setDate(day2.getDate() + 3)
	enabledDates.push(day2)
	const day3 = new Date(day2)
	day3.setDate(day3.getDate() + 3)
	enabledDates.push(day3)
	const day4 = new Date(day2)
	day4.setMonth(day4.getMonth() - 1)
	enabledDates.push(day4)
	const day5 = new Date(day2)
	day5.setFullYear(day5.getFullYear() - 1)
	enabledDates.push(day5)
</script>

<Split>
	{#snippet left()}
		<div class="left">
			<DatePicker
				bind:value
				bind:min
				bind:max
				locale={locale.value}
				bind:browseWithoutSelecting
				{timePrecision}
				{disabledDates}
			/>
		</div>
	{/snippet}
	{#snippet right()}
		<div>
			<h3 class="no-top">Props</h3>
			<Prop label="value">{value}</Prop>
			<Prop label="min" bind:value={min} />
			<Prop label="max" bind:value={max} />
			<Prop label="locale" bind:value={locale} values={locales} />
			<Prop label="browseWithoutSelecting" bind:value={browseWithoutSelecting} />
			<Prop
				label="timePrecision"
				bind:value={timePrecision}
				values={[null, 'minute', 'second', 'millisecond']}>{timePrecision}</Prop
			>
			{#each disabledDates as day, index}
				<Prop label="disabledDate {index + 1}" value={day} />
			{/each}
		</div>
	{/snippet}
</Split>

<h3 class="no-top">DatePicker (enabledDates)</h3>
<Split>
	{#snippet left()}
		<div class="left">
			<DatePicker
				bind:value
				bind:min
				bind:max
				locale={locale.value}
				bind:browseWithoutSelecting
				{timePrecision}
				{enabledDates}
			/>
		</div>
	{/snippet}
	{#snippet right()}
		<div>
			<h3 class="no-top">Props</h3>
			<Prop label="value">{value}</Prop>
			<Prop label="min" bind:value={min} />
			<Prop label="max" bind:value={max} />
			<Prop label="locale">date-fns <code>hy</code></Prop>
			<Prop label="browseWithoutSelecting" bind:value={browseWithoutSelecting} />
			<Prop
				label="timePrecision"
				bind:value={timePrecision}
				values={[null, 'minute', 'second', 'millisecond']}>{timePrecision}</Prop
			>
			{#each enabledDates as day, index}
				<Prop label="enableDate {index + 1}" value={day} />
			{/each}
		</div>
	{/snippet}
</Split>

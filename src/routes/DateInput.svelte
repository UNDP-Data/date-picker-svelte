<script lang="ts">
	import DateInput from '$lib/DateInput.svelte'
	import Prop from './prop.svelte'
	import Split from './split.svelte'
	import { localeFromDateFnsLocale } from '$lib'
	import { hy, de, nb } from 'date-fns/locale'

	let id: string = $state('')
	let placeholder: string = $state('')
	let value: Date | undefined = $state()
	let min: Date | undefined = $state()
	let max: Date | undefined = $state()
	let valid: boolean = $state(false)
	let visible: boolean = $state(false)
	let disabled: boolean = $state(false)
	let required: boolean = $state(false)
	let closeOnSelection: boolean = $state(false)
	let browseWithoutSelecting: boolean = $state(false)
	let format: string = $state('')
	let dynamicPositioning: boolean = $state(true)
	let timePrecision: 'minute' | 'second' | 'millisecond' | null = $state(null)
	let disabledDay1 = new Date()
	let disabledDay2 = new Date(disabledDay1)
	disabledDay2.setDate(disabledDay1.getDate() + 1)
	let disabledDates = $state([disabledDay1, disabledDay2].filter((d) => d !== undefined))
	let locales = [
		{ key: 'default', value: localeFromDateFnsLocale({}) },
		{ key: 'nb (date-fns)', value: localeFromDateFnsLocale(nb) },
		{ key: 'de (date-fns)', value: localeFromDateFnsLocale(de) },
		{ key: 'hy (date-fns)', value: localeFromDateFnsLocale(hy) },
	]
	let locale = $state(locales[0])
</script>

<Split>
	{#snippet left()}
		<DateInput
			bind:id
			bind:value
			bind:min
			bind:max
			bind:placeholder
			bind:valid
			bind:format
			bind:visible
			bind:disabled
			bind:required
			bind:closeOnSelection
			bind:browseWithoutSelecting
			bind:dynamicPositioning
			bind:timePrecision
			bind:disabledDates
			bind:locale={locale.value}
		/>
	{/snippet}

	{#snippet right()}
		<h3 class="no-top">Props</h3>
		<Prop label="value">{value}</Prop>
		<Prop label="min" bind:value={min} />
		<Prop label="max" bind:value={max} />
		<Prop label="id" bind:value={id} />
		<Prop label="placeholder" bind:value={placeholder} />
		<Prop label="valid" bind:value={valid} />
		<Prop label="format" bind:value={format} />
		<Prop label="visible" bind:value={visible} />
		<Prop label="disabled" bind:value={disabled} />
		<Prop label="required" bind:value={required} />
		<Prop label="closeOnSelection" bind:value={closeOnSelection} />
		<Prop label="browseWithoutSelecting" bind:value={browseWithoutSelecting} />
		<Prop label="dynamicPositioning" bind:value={dynamicPositioning} />
		<Prop label="locale" bind:value={locale} values={locales} />
		<Prop
			label="timePrecision"
			bind:value={timePrecision}
			values={[null, 'minute', 'second', 'millisecond']}>{timePrecision}</Prop
		>
		{#each disabledDates as day, index}
			<Prop label="disabledDate {index + 1}" value={day} />
		{/each}
	{/snippet}
</Split>

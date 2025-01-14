<script>
	import { createDatePicker, melt } from '@melt-ui/svelte';
	import { ChevronRight, ChevronLeft, Calendar } from 'lucide-svelte';
	import { fade } from 'svelte/transition';

	const monthNamesMap = [
		'Jan',
		'Feb',
		'Mar',
		'Apr',
		'May',
		'Jun',
		'Jul',
		'Aug',
		'Sep',
		'Oct',
		'Nov',
		'Dec'
	];

	const {
		elements: {
			calendar,
			cell,
			content,
			field,
			grid,
			heading,
			label,
			nextButton,
			prevButton,
			segment,
			trigger
		},
		states: { months, headingValue, weekdays, segmentContents, open, value },
		helpers: { isDateDisabled, isDateUnavailable }
	} = createDatePicker({
		forceVisible: true,
		fixedWeeks: true,
		numberOfMonths: 12,
		pagedNavigation: true
	});

	$: console.log('$months', $months);
</script>

<div class="mt-20 flex w-full flex-col items-center gap-3">
	<div>
		<span use:melt={$label}>Date</span>
		<div use:melt={$field}>
			{#each $segmentContents as seg}
				<div use:melt={$segment(seg.part)}>
					{seg.value}
				</div>
			{/each}
			<div class="button-container">
				<button use:melt={$trigger}>
					<Calendar size={16} />
				</button>
			</div>
		</div>
	</div>

	<div class="text-gray-100">You selected: {$value}</div>
	{#if $open}
		<div transition:fade={{ duration: 100 }} use:melt={$content}>
			<div use:melt={$calendar}>
				<header>
					<button use:melt={$prevButton}>
						<ChevronLeft size={24} />
					</button>
					<div use:melt={$heading}>
						{$headingValue}
					</div>
					<button use:melt={$nextButton}>
						<ChevronRight size={24} />
					</button>
				</header>

				<div class="!grid grid-cols-4 gap-1">
					{#each $months as month, index}
						<div
							class="!w-full rounded-lg px-2"
							use:melt={$cell(
								month.dates.find((date) => date.day == 1),
								month.value
							)}
						>
							{monthNamesMap[index]}
						</div>
					{/each}
				</div>
			</div>
		</div>
	{/if}
</div>

<style lang="postcss">
	.button-container {
		@apply ml-4 flex w-full items-center justify-end;
	}

	[data-melt-popover-content] {
		@apply z-10 min-w-[320px] rounded-lg bg-neutral-900 shadow-sm;
	}

	[data-melt-popover-trigger] {
		@apply rounded-md bg-magnum-400 p-1 text-neutral-950 transition-all hover:bg-magnum-400/80;
	}

	[data-melt-datefield-label] {
		@apply select-none font-medium text-white;
	}

	[data-melt-datefield-label][data-invalid] {
		@apply text-red-500;
	}

	[data-melt-datefield-field] {
		@apply mt-1.5 flex w-full min-w-[200px] items-center rounded-lg border border-magnum-400/60 bg-neutral-800/80 p-1.5 text-magnum-400;
	}

	[data-melt-datefield-field][data-invalid] {
		@apply border-red-400;
	}

	[data-melt-datefield-segment][data-invalid] {
		@apply text-red-500;
	}

	[data-melt-datefield-segment]:not([data-segment='literal']) {
		@apply px-0.5;
	}

	[data-melt-datefield-validation] {
		@apply self-start text-red-500;
	}

	[data-melt-calendar] {
		@apply w-full rounded-lg bg-neutral-800/90 p-3 text-white shadow-sm;
	}

	header {
		@apply flex items-center justify-between pb-2;
	}

	header + div {
		@apply flex items-center gap-6;
	}

	[data-melt-calendar-prevbutton] {
		@apply rounded-lg p-1 transition-all hover:bg-magnum-500/20;
	}

	[data-melt-calendar-nextbutton] {
		@apply rounded-lg p-1 transition-all hover:bg-magnum-500/20;
	}

	[data-melt-calendar-prevbutton][data-disabled] {
		@apply pointer-events-none rounded-lg p-1 opacity-40;
	}

	[data-melt-calendar-nextbutton][data-disabled] {
		@apply pointer-events-none rounded-lg p-1 opacity-40;
	}

	[data-melt-calendar-heading] {
		@apply font-semibold;
	}

	th {
		@apply text-sm font-semibold;

		& div {
			@apply flex h-6 w-6 items-center justify-center p-4;
		}
	}

	[data-melt-calendar-grid] {
		@apply w-full;
	}

	[data-melt-calendar-cell] {
		@apply flex h-6 w-6 cursor-pointer select-none items-center justify-center rounded-lg p-4 hover:bg-magnum-500/20 focus:ring focus:ring-magnum-400;
	}

	[data-melt-calendar-cell][data-disabled] {
		@apply pointer-events-none opacity-40;
	}

	[data-melt-calendar-cell][data-unavailable] {
		@apply pointer-events-none text-red-400 line-through;
	}

	[data-melt-calendar-cell][data-selected] {
		@apply bg-magnum-400 text-neutral-950;
	}

	[data-melt-calendar-cell][data-outside-visible-months] {
		@apply pointer-events-none cursor-default opacity-40 hover:bg-transparent;
	}

	[data-melt-calendar-cell][data-outside-month] {
		@apply pointer-events-none cursor-default opacity-0 hover:bg-transparent;
	}
</style>

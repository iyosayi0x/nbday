<script lang="ts">
	import { goto } from '$app/navigation';
	import Icon from '@iconify/svelte';
	const PERSISTENCE_KEY_NAME = 'PREFLIGHT_CHECKLIST';

	/**
	 * state
	 */
	let preflightTasks = $state([
		{
			id: '1',
			label: 'Maximize your browser window',
			isDone: false
		},
		{
			id: '2',
			label: 'Enable audio',
			hint: 'Best with headphones 🎧',
			isDone: false
		},
		{
			id: '3',
			label: 'Prepare for a walk down memory lane',
			isDone: false
		}
	]);

	/**
	 * methods
	 */
	const handleTaskDone = (id: string, checked: boolean) => {
		preflightTasks = preflightTasks.map((task) => {
			if (task.id === id) {
				return { ...task, isDone: checked };
			}
			return task;
		});

		localStorage.setItem(PERSISTENCE_KEY_NAME, JSON.stringify(preflightTasks));
	};

	const handleEnterStory = () => {
		goto('/story');
	};

	const allTasksDone = $derived.by(() => {
		return preflightTasks.every((task) => task.isDone);
	});

	/**
	 * side effects
	 */
	$effect(() => {
		const storedTasks = localStorage.getItem(PERSISTENCE_KEY_NAME);
		if (storedTasks) {
			preflightTasks = JSON.parse(storedTasks);
		}
	});
</script>

<div class="page-wrapper">
	<!-- Background decorative elements -->
	<div class="ambient-bg fixed inset-0 z-0" aria-hidden="true"></div>
	<div
		class="pointer-events-none fixed top-[-20%] left-[-10%] h-[50vw] w-[50vw] rounded-full bg-gold/5 blur-[120px]"
		aria-hidden="true"
	></div>
	<div
		class="pointer-events-none fixed right-[-10%] bottom-[-10%] h-[40vw] w-[40vw] rounded-full bg-gold/10 blur-[100px]"
		aria-hidden="true"
	></div>

	<!-- Main Content -->
	<main class="relative z-10 flex min-h-screen w-full flex-col items-center justify-center p-4">
		<!-- Preflight Modal -->
		<div
			class="glass-panel flex w-full max-w-lg transform flex-col items-center rounded-xl p-8 text-center transition-all duration-700 md:p-12"
			role="dialog"
			aria-labelledby="modal-title"
			aria-describedby="modal-description"
		>
			<!-- Header -->
			<header class="mb-10 space-y-3">
				<div
					class="mb-4 inline-flex h-12 w-12 items-center justify-center rounded-full border border-gold/20 bg-gold/10"
					aria-hidden="true"
				>
					<Icon icon="ph:list-bullets" class="material-symbols-outlined text-2xl text-gold"></Icon>
				</div>
				<h1 id="modal-title" class="text-3xl tracking-wide text-ghost md:text-4xl">
					Setting the Stage
				</h1>
				<p
					id="modal-description"
					class="mx-auto max-w-xs text-sm font-light text-white/60 md:text-base"
				>
					For the best experience, please complete the preparation ritual below.
				</p>
			</header>

			<!-- Preflight Checklist -->
			<section
				class="mb-10 w-full space-y-5 pl-2 text-left md:pl-8"
				aria-label="Preflight checklist"
			>
				{#each preflightTasks as task}
					<label class="custom-checkbox group flex cursor-pointer items-center gap-4 select-none">
						<input
							type="checkbox"
							class="peer hidden"
							aria-label="Maximize your browser window"
							checked={task.isDone}
							onchange={(e) => {
								const evt = e as unknown as Event & { target: { checked: boolean } };
								handleTaskDone(task.id, evt.target.checked);
							}}
						/>
						<div
							class="flex h-6 w-6 items-center justify-center rounded-full border-2 border-gold/50 shadow-[0_0_10px_rgba(212,175,53,0.1)] transition-all duration-300 group-hover:border-gold peer-checked:border-gold peer-checked:bg-gold"
							aria-hidden="true"
						>
							<svg
								class="hidden h-4 w-4 font-bold text-black"
								fill="none"
								stroke="currentColor"
								stroke-width="3"
								viewBox="0 0 24 24"
								aria-hidden="true"
							>
								<path d="M5 13l4 4L19 7" stroke-linecap="round" stroke-linejoin="round"></path>
							</svg>
						</div>
						<span class="text-base font-light tracking-wide text-ghost transition-colors">
							{task.label}
							{#if task.hint}
								<span class="ml-1 text-sm text-white/40">({task.hint})</span>
							{/if}
						</span>
					</label>
				{/each}
			</section>

			<!-- Call to Action -->
			<button
				type="button"
				class="group shadow-glow relative overflow-hidden rounded-full bg-gold px-8 py-3.5 transition-all duration-300 hover:scale-105 hover:shadow-[0_0_30px_rgba(212,175,53,0.5)]"
				aria-label="Enter the story"
				disabled={!allTasksDone}
				onclick={handleEnterStory}
			>
				<div
					class="absolute inset-0 translate-y-full bg-white/20 transition-transform duration-300 ease-out group-hover:translate-y-0"
					aria-hidden="true"
				></div>
				<span
					class="text-background-dark relative flex items-center gap-2 text-sm font-bold tracking-wider uppercase"
				>
					<span>Enter the Story</span>
					<Icon icon="ph:arrow-right-light" class="text-[20px] font-bold" aria-hidden="true">
						arrow_forward
					</Icon>
				</span>
			</button>

			<!-- Footer Note -->
			<footer class="mt-8 w-full border-t border-white/5 pt-4">
				<p class="text-center text-xs font-normal text-white/20">
					Experience designed for desktop viewing
				</p>
			</footer>
		</div>
	</main>
</div>

<style>
	/* Checkbox States */
	.custom-checkbox input:checked + div {
		background-color: #d4af35;
		border-color: #d4af35;
	}

	.custom-checkbox input:checked + div svg {
		display: block;
	}

	.custom-checkbox input:checked ~ span {
		text-decoration: line-through;
		text-decoration-color: #d4af35;
		color: #9ca3af;
		transition: all 0.3s ease;
	}

	/* Glass Panel Effect */
	.glass-panel {
		background: rgba(255, 255, 255, 0.03);
		backdrop-filter: blur(16px);
		-webkit-backdrop-filter: blur(16px);
		border: 1px solid rgba(255, 255, 255, 0.08);
		box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
	}

	/* Animated Background */
	@keyframes drift {
		0% {
			background-position: 0% 50%;
		}
		50% {
			background-position: 100% 50%;
		}
		100% {
			background-position: 0% 50%;
		}
	}

	.ambient-bg {
		background: radial-gradient(circle at 50% 50%, #1a1810 0%, #0a0a0a 100%);
	}
</style>

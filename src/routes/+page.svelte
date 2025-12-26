<script>
	import { goto } from '$app/navigation';

	const onBeginJourney = () => {
		goto('/preflight');
	};
</script>

<div class="flex h-screen justify-center overflow-hidden">
	<!-- ---------- main --------- -->
	<main class="flex flex-col items-center justify-center gap-20">
		<div class="flex flex-col items-center justify-center gap-8">
			<h1 class="text-center text-4xl font-bold md:text-7xl">
				The Legend of <span class="text-gold">Nsukka SpiderWoman</span>
			</h1>
			<p class="w-4xl max-w-[90vw] text-center leading-6 text-ghost">
				Some lives are lived in chapters; others are written in the stars. This is a journey through
				the moments that defined a quarter century the laughter, the chaos and the quiet magic in
				between
			</p>
		</div>

		<div>
			<button
				class="h-16 w-64 animate-pulse cursor-pointer rounded-full border border-gold font-bold"
				onclick={onBeginJourney}
			>
				BEGIN THE JOURNEY
			</button>
		</div>
	</main>

	<!-- ------ circle display -----  -->
	<aside
		class="home__floating-bubble fixed -right-20 -bottom-32 h-[600px] w-[600px] rounded-full"
	></aside>
</div>

<style>
	.home__floating-bubble {
		background: linear-gradient(150deg, transparent, var(--color-gold-glow));
		filter: blur(80px);
		opacity: 0.8;
	}
</style>

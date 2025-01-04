<script>

    import { SlideToggle } from "@skeletonlabs/skeleton";

</script>
<div class="h-full w-full flex">
	<div class="w-full p-5 space-y-5">
		<h3 class="h3">Brute force durch Criminal Thinking</h3>
		<div class="card p-4 space-y-2">
			<p>Das zu knackende Passwort eingeben:</p>
			<div class="input-group input-group-divider grid-cols-[auto_1fr_auto]">
				<div class="input-group-shim"><svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32"><path fill="currentColor" d="M6.5 6A4.5 4.5 0 0 0 2 10.5v11A4.5 4.5 0 0 0 6.5 26h19a4.5 4.5 0 0 0 4.5-4.5v-11A4.5 4.5 0 0 0 25.5 6zM4 10.5A2.5 2.5 0 0 1 6.5 8h19a2.5 2.5 0 0 1 2.5 2.5v11a2.5 2.5 0 0 1-2.5 2.5h-19A2.5 2.5 0 0 1 4 21.5zm3.707 2.793a1 1 0 0 0-1.414 1.414L7.586 16l-1.293 1.293a1 1 0 1 0 1.414 1.414L9 17.414l1.293 1.293a1 1 0 0 0 1.414-1.414L10.414 16l1.293-1.293a1 1 0 0 0-1.414-1.414L9 14.586zm6.086 0a1 1 0 0 1 1.414 0l1.293 1.293l1.293-1.293a1 1 0 0 1 1.414 1.414L17.914 16l1.293 1.293a1 1 0 0 1-1.414 1.414L16.5 17.414l-1.293 1.293a1 1 0 0 1-1.414-1.414L15.086 16l-1.293-1.293a1 1 0 0 1 0-1.414M22 17a1 1 0 1 0 0 2h3a1 1 0 1 0 0-2z"/></svg></div>
				<input type="search" placeholder="123123" />
				<button class="variant-filled-secondary">Knacken</button>
			</div>
		</div>
	</div>
	<div class="h-full overflow-clip">
		<span class="divider-vertical border-dashed h-full"/>
	</div>
	<div class="w-full p-5 space-y-5">
		<h3 class="h3">Schutz durch Creative Thinking</h3>
		
		<div class="space-y-2">
			<h4 class="h4">User Seite</h4>
			<div class="card p-4 flex flex-col space-y-2 w-max">
				<SlideToggle name="slider-label" active="bg-primary-500" size="sm">Passwort Länge</SlideToggle>
				<SlideToggle name="slider-label" active="bg-primary-500" size="sm">Passwort alpha Numerik</SlideToggle>
				<SlideToggle name="slider-label" active="bg-primary-500" size="sm">Passwort Sonderzeichen</SlideToggle>
			</div>
		</div>

		<div class="space-y-2">
			<h4 class="h4">Anbieter Seite</h4>
			<div class="card p-4 flex flex-col space-y-2 w-max">
				<SlideToggle name="slider-label" active="bg-primary-500" size="sm">Maximale Versuche</SlideToggle>
				<SlideToggle name="slider-label" active="bg-primary-500" size="sm">Captcha Anfordern</SlideToggle>
			</div>
		</div>
	</div>
</div>

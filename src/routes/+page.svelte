<script lang="ts">
    import { FileDropzone, SlideToggle } from "@skeletonlabs/skeleton";

	let setPassword = "";
	let currentPassword = "";
	let files: FileList;
	let fileAsText:string[];

	let passwordFound:boolean|undefined = undefined;
	let delay = false;

	let passwordLength = false;
	let passwordAlphaNumeric = false;
	let passwordSpecialCharacters = false;
	let passwordTries = false;
	let passwordCaptcha = false;

	let timerId: number | null = null;
	let elapsedTime: number = 0;

	let startTime: number | null = null;

	async function startTimer() {
		if (!timerId) {
		startTime = Date.now() - elapsedTime;
		timerId = window.setInterval(() => {
			if (startTime !== null) {
			elapsedTime = Date.now() - startTime;
			}
		}, 10);
		}
	}

	async function stopTimer() {
		if (timerId !== null) {
		clearInterval(timerId);
		timerId = null;
		}
	}

	function resetTimer() {
		stopTimer();
		elapsedTime = 0;
		startTime = null;
	}

	async function hack() {
		if(!fileAsText)
			return;

		passwordFound = undefined;
		resetTimer();
		await startTimer();

		searchPassword();
	}

	async function searchPassword() {
		for (let index = 0; index < fileAsText.length; index++) {
			const element = fileAsText[index];
			currentPassword = element;
			if(element === setPassword) {
				stopTimer();
				passwordFound = true;
				return;
			}

			if(delay)
				await delay1ms(1);
		}

		stopTimer();
		passwordFound = false;
	}

	function delay1ms(ms: number) {
		return new Promise( resolve => setTimeout(resolve, ms) );
	}

	function onChangeHandler(e: Event): void {
		const fileReader = new FileReader();
		fileReader.readAsText(files[0]);

		fileReader.onload = () => {
			const fileContent = fileReader.result as string;
			fileAsText = fileContent.split("\n");
		};
	}

	$: seconds = Math.floor(elapsedTime / 1000); // Whole seconds
	$: milliseconds = elapsedTime % 1000; // Remaining milliseconds
</script>


<div class="h-full w-full flex items-center">
	<div class="w-full p-5 space-y-5 items-center justify-items-center">
		<h3 class="h3">Brute force durch Criminal Thinking</h3>

		<div class="space-y-5 card p-5 w-max">
			<div class="space-y-2">
				<h4 class="h4">Passwort knacken</h4>
				<div class="card p-4 space-y-2 w-max">
					<div class="space-y-2">
						<p>Das zu knackende Passwort eingeben:</p>
						<div class="flex justify-between">
							<div class="input-group input-group-divider grid-cols-[auto_1fr_auto] w-max {timerId?"pointer-events-none":""}">
								<div class="input-group-shim"><svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32"><path fill="currentColor" d="M6.5 6A4.5 4.5 0 0 0 2 10.5v11A4.5 4.5 0 0 0 6.5 26h19a4.5 4.5 0 0 0 4.5-4.5v-11A4.5 4.5 0 0 0 25.5 6zM4 10.5A2.5 2.5 0 0 1 6.5 8h19a2.5 2.5 0 0 1 2.5 2.5v11a2.5 2.5 0 0 1-2.5 2.5h-19A2.5 2.5 0 0 1 4 21.5zm3.707 2.793a1 1 0 0 0-1.414 1.414L7.586 16l-1.293 1.293a1 1 0 1 0 1.414 1.414L9 17.414l1.293 1.293a1 1 0 0 0 1.414-1.414L10.414 16l1.293-1.293a1 1 0 0 0-1.414-1.414L9 14.586zm6.086 0a1 1 0 0 1 1.414 0l1.293 1.293l1.293-1.293a1 1 0 0 1 1.414 1.414L17.914 16l1.293 1.293a1 1 0 0 1-1.414 1.414L16.5 17.414l-1.293 1.293a1 1 0 0 1-1.414-1.414L15.086 16l-1.293-1.293a1 1 0 0 1 0-1.414M22 17a1 1 0 1 0 0 2h3a1 1 0 1 0 0-2z"/></svg></div>
								<input type="search" placeholder="123123" bind:value={setPassword}/>
								<button class="variant-filled-secondary" on:click={hack}>
									{#if timerId}
										<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="currentColor" stroke-dasharray="16" stroke-dashoffset="16" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3c4.97 0 9 4.03 9 9"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.2s" values="16;0"/><animateTransform attributeName="transform" dur="1.5s" repeatCount="indefinite" type="rotate" values="0 12 12;360 12 12"/></path></svg>
									{:else}
										Knacken
									{/if}
								</button>
							</div>
							<div class="flex items-center">
								<SlideToggle name="slider-label" active="bg-primary-500" size="sm" bind:checked={delay}>1ms Verzögerung</SlideToggle>
							</div>
						</div>
					</div>

					<div class="flex space-x-2 w-full pointer-events-none">
						<div class="space-y-2">
							<p>Jetziger Versuch</p>
							<div class="input-group input-group-divider grid-cols-[1fr_auto]">
								<input type="text" placeholder="" bind:value={currentPassword}/>
								<div><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 16 16"><path fill="currentColor" d="M3 8.5a1 1 0 1 0 0-2a1 1 0 0 0 0 2"/><path fill="currentColor" fill-rule="evenodd" d="M4.5 3c1.56 0 2.94.794 3.75 2h5.26a1 1 0 0 1 .807.409l1.49 2.04a.99.99 0 0 1 .033 1.13l-1.26 1.95a.997.997 0 0 1-1.41.276L12.02 10l-1.19.812a1 1 0 0 1-1.13 0L8.51 10h-.258c-.808 1.21-2.18 2-3.75 2a4.5 4.5 0 0 1 0-9zm3.75 6a1 1 0 0 0-.832.444a3.5 3.5 0 0 1-2.91 1.56c-1.93 0-3.5-1.57-3.5-3.5s1.57-3.5 3.5-3.5c1.21 0 2.28.616 2.91 1.56c.186.277.498.444.832.444h5.26L15 8.048l-1.26 1.95l-1.15-.805a1 1 0 0 0-1.14-.005L10.26 10l-1.19-.812a1 1 0 0 0-.566-.175h-.258z" clip-rule="evenodd"/></svg></div>
							</div>
						</div>
		
						<div class="space-y-2 w-40">
							<p>Verstrichene Zeit</p>
							<div class="input-group input-group-divider grid-cols-[1fr_auto]">
								<div class="h-10">{seconds}.{milliseconds.toString().padStart(3, '0')}</div>
								<div><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><g fill="none"><path d="m12.593 23.258l-.011.002l-.071.035l-.02.004l-.014-.004l-.071-.035q-.016-.005-.024.005l-.004.01l-.017.428l.005.02l.01.013l.104.074l.015.004l.012-.004l.104-.074l.012-.016l.004-.017l-.017-.427q-.004-.016-.017-.018m.265-.113l-.013.002l-.185.093l-.01.01l-.003.011l.018.43l.005.012l.008.007l.201.093q.019.005.029-.008l.004-.014l-.034-.614q-.005-.018-.02-.022m-.715.002a.02.02 0 0 0-.027.006l-.006.014l-.034.614q.001.018.017.024l.015-.002l.201-.093l.01-.008l.004-.011l.017-.43l-.003-.012l-.01-.01z"/><path fill="currentColor" d="M12 2c5.523 0 10 4.477 10 10s-4.477 10-10 10S2 17.523 2 12S6.477 2 12 2m0 2a8 8 0 1 0 0 16a8 8 0 0 0 0-16m0 2a1 1 0 0 1 .993.883L13 7v4.586l2.707 2.707a1 1 0 0 1-1.32 1.497l-.094-.083l-3-3a1 1 0 0 1-.284-.576L11 12V7a1 1 0 0 1 1-1"/></g></svg></div>
							</div>
						</div>

						{#if passwordFound == undefined}
							<div class="flex items-end w-48">
								<a href="/" class="btn variant-filled-surface">
									<span><svg xmlns="http://www.w3.org/2000/svg" width="15" height="16" viewBox="0 0 15 16"><path fill="currentColor" d="M7.5 10c-.28 0-.5-.22-.5-.5v-7c0-.28.22-.5.5-.5s.5.22.5.5v7c0 .28-.22.5-.5.5"/><path fill="currentColor" d="M7.5 15C3.92 15 1 12.18 1 8.72c0-2.41 1.46-4.64 3.72-5.68c.25-.11.55 0 .66.25s0 .55-.25.66c-1.91.87-3.14 2.74-3.14 4.77c0 2.91 2.47 5.28 5.5 5.28s5.5-2.37 5.5-5.28c0-2.02-1.23-3.9-3.14-4.77a.5.5 0 0 1-.25-.66c.11-.25.41-.36.66-.25c2.26 1.03 3.72 3.26 3.72 5.68c0 3.46-2.92 6.28-6.5 6.28Z"/></svg></span>
									<span>Undefiniert</span>
								</a>
							</div>
						{:else if passwordFound}
							<div class="flex items-end w-48">
								<a href="/" class="btn variant-filled-success">
									<span><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><g fill="currentColor"><path d="M7.665 10.237L9.198 8.95l1.285 1.532l3.064-2.571l1.286 1.532l-4.596 3.857z"/><path fill-rule="evenodd" d="M16.207 4.893a8 8 0 0 1 .662 10.565q.023.02.045.042l4.243 4.243a1 1 0 0 1-1.414 1.414L15.5 16.914l-.042-.045A8.001 8.001 0 0 1 4.893 4.893a8 8 0 0 1 11.314 0m-1.414 9.9a6 6 0 1 0-8.485-8.485a6 6 0 0 0 8.485 8.485" clip-rule="evenodd"/></g></svg></span>
									<span>Gefunden</span>
								</a>
							</div>
						{:else}
							<div class="flex items-end w-48">
								<a href="/" class="btn variant-filled-error">
									<span><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="m19.6 21l-6.3-6.3q-.75.6-1.725.95T9.5 16q-2.725 0-4.612-1.888T3 9.5t1.888-4.612T9.5 3t4.613 1.888T16 9.5q0 1.1-.35 2.075T14.7 13.3l6.3 6.3zM9.5 14q1.875 0 3.188-1.312T14 9.5t-1.312-3.187T9.5 5T6.313 6.313T5 9.5t1.313 3.188T9.5 14"/></svg></span>
									<span>Nicht gefunden</span>
								</a>
							</div>	
						{/if}
					</div>
				</div>
			</div>
	
			<div class="space-y-2">
				<h4 class="h4">Rainbow table</h4>
				<div class="card p-4 space-y-2 w-max">
					<p>Liste an bekannten passwörtern anhängen:</p>
					<FileDropzone name="files" bind:files={files} on:change={onChangeHandler}>
						<svelte:fragment slot="lead"><svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" viewBox="0 0 256 256" class="mx-auto"><path fill="currentColor" d="M48 120a8 8 0 0 0 8-8V40h88v48a8 8 0 0 0 8 8h48v16a8 8 0 0 0 16 0V88a8 8 0 0 0-2.34-5.66l-56-56A8 8 0 0 0 152 24H56a16 16 0 0 0-16 16v72a8 8 0 0 0 8 8m112-68.69L188.69 80H160Zm-5.49 105.34L137.83 180l16.68 23.35a8 8 0 0 1-13 9.3L128 193.76l-13.49 18.89a8 8 0 1 1-13-9.3L118.17 180l-16.68-23.35a8 8 0 1 1 13-9.3L128 166.24l13.49-18.89a8 8 0 0 1 13 9.3ZM92 152a8 8 0 0 1-8 8H72v48a8 8 0 0 1-16 0v-48H44a8 8 0 0 1 0-16h40a8 8 0 0 1 8 8m128 0a8 8 0 0 1-8 8h-12v48a8 8 0 0 1-16 0v-48h-12a8 8 0 0 1 0-16h40a8 8 0 0 1 8 8"/></svg></svelte:fragment>
						<svelte:fragment slot="message">Passwort Dokument hochladen</svelte:fragment>
						<svelte:fragment slot="meta">.txt Dokument seperiert durch neue Zeilen<br>{files?.length > 0 ? files[0].name : "Bitte lade ein Dokument hoch"}</svelte:fragment>
					</FileDropzone>
				</div>
			</div>
		</div>

	</div>
	<div class="h-full overflow-clip">
		<span class="divider-vertical border-dashed h-full"/>
	</div>
	<div class="w-full p-5 space-y-5 items-center justify-items-center">
		<h3 class="h3">Schutz durch Creative Thinking</h3>
		
		<div class="space-y-5 card p-5 w-max">
			<div class="space-y-2">
				<h4 class="h4">User Seite</h4>
				<div class="card p-4 flex flex-col space-y-2 w-max">
					<SlideToggle name="slider-label" active="bg-primary-500" size="sm" bind:checked={passwordLength}>Passwort Länge</SlideToggle>
					<SlideToggle name="slider-label" active="bg-primary-500" size="sm" bind:checked={passwordAlphaNumeric}>Passwort alpha Numerik</SlideToggle>
					<SlideToggle name="slider-label" active="bg-primary-500" size="sm" bind:checked={passwordSpecialCharacters}>Passwort Sonderzeichen</SlideToggle>
				</div>
			</div>
	
			<div class="space-y-2">
				<h4 class="h4">Anbieter Seite</h4>
				<div class="card p-4 flex flex-col space-y-2 w-max">
					<SlideToggle name="slider-label" active="bg-primary-500" size="sm" bind:checked={passwordTries}>Maximale Versuche</SlideToggle>
					<SlideToggle name="slider-label" active="bg-primary-500" size="sm" bind:checked={passwordCaptcha}>Captcha Anfordern</SlideToggle>
					<a href="/" class="btn variant-filled">
						<span><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="M12.003 21q-1.866 0-3.5-.701q-1.632-.701-2.854-1.912t-1.936-2.85T3 12.04h1q0 1.65.635 3.102q.634 1.453 1.722 2.54T8.9 19.398t3.099.628q3.35 0 5.675-2.325T20 12.025T17.675 6.35T12 4.025q-2.436 0-4.365 1.28q-1.927 1.28-2.881 3.349h2.9v1H3V5h1v2.98q1.087-2.228 3.21-3.604T12 3q1.868 0 3.51.709t2.858 1.922t1.923 2.857t.709 3.509t-.708 3.51t-1.924 2.859t-2.856 1.925t-3.509.709M10 15.77q-.376 0-.63-.255t-.254-.63v-3q0-.376.287-.63q.287-.255.712-.255V9.884q0-.777.554-1.33Q11.222 8 12 8t1.331.554t.554 1.33V11q.425 0 .712.254q.288.255.288.63v3q0 .377-.255.631q-.254.254-.63.254zm.885-4.77h2.23V9.892q0-.47-.325-.797T12 8.77t-.79.326t-.326.797z"/></svg></span>
						<span>Sperrung zurücksetzten</span>
					</a>
				</div>
			</div>
		</div>
	</div>
</div>
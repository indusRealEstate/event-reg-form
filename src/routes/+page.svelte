<script lang="ts">
	import { v4 as uuidv4 } from 'uuid';

	import HeaderImg from '$lib/images/bayz-header.avif';
	import MainImg from '$lib/images/bayz-main.avif';
	import Dialog from '$lib/components/dialog/Dialog.svelte';

	let name = '';
	let email = '';
	let contactNo = '';
	let source = '';

	let formSubmitted = false;

	let formJson: any;

	let otherSource = '';

	// @ts-ignore
	const dataSubmit = async () => {
		const uuid = uuidv4();
		formJson = {
			name: name,
			email: email,
			contactNo: contactNo,
			source: source == 'Others' ? otherSource : source,
			uid: uuid
		};

		formSubmitted = true;
	};
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	{#if formSubmitted}
		<Dialog {formJson} />
	{/if}
	<div class="flex justify-center">
		<form on:submit|preventDefault={dataSubmit}>
			<div class="space-y-12 border-solid border-2 border-red-400 p-8 mt-8 rounded-md mb-8">
				<div class=" max-w-4xl">
					<img
						src={HeaderImg}
						alt="header-img"
						class="relative z-10 object-contain w-full rounded"
					/>
					<h2 class="mt-6 text-4xl font-semibold leading-9 text-red-600">Jaipur, get ready!</h2>
					<p class="mt-4 text-xl leading-7 text-gray-600">
						Seize the chance to Invest in the most Dynamic Real Estate Market. Register now and get
						a chance to win a free trip to Dubai or a gold coin.
					</p>

					<div class="mt-10 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-6">
						<div class="sm:col-span-5">
							<label for="username" class="block text-xl font-medium leading-6 text-gray-900"
								>Name <span class="text-red-600">*</span></label
							>
							<div class="mt-2">
								<input
									required
									bind:value={name}
									type="text"
									name="name"
									id="name"
									class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-red-600 xl:text-xl xl:leading-10"
								/>
							</div>
						</div>

						<div class="sm:col-span-5">
							<label for="username" class="block text-xl font-medium leading-6 text-gray-900"
								>Email <span class="text-red-600">*</span></label
							>
							<div class="mt-2">
								<input
									required
									bind:value={email}
									type="email"
									name="email"
									id="email"
									class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-red-600 xl:text-xl xl:leading-10"
								/>
							</div>
						</div>

						<div class="sm:col-span-5">
							<label for="username" class="block text-xl font-medium leading-6 text-gray-900"
								>Contact Number <span class="text-red-600">*</span></label
							>
							<div class="mt-2">
								<input
									required
									bind:value={contactNo}
									type="text"
									name="name"
									id="name"
									class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-red-600 xl:text-xl xl:leading-10"
								/>
							</div>
						</div>
						<div class="sm:col-span-5">
							<label for="username" class="block text-xl font-medium leading-6 text-gray-900"
								>How did you hear about us? <span class="text-red-600">*</span></label
							>
							<div class="mt-2">
								<select
									required
									bind:value={source}
									id="country"
									name="country"
									autocomplete="country-name"
									class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-red-600 sm:max-w-xs xl:text-xl xl:leading-10"
								>
									<option>Google</option>
									<option>Facebook</option>
									<option>Newspaper</option>
									<option>Instagram</option>
									<option>LinkedIn</option>
									<option>Others</option>
								</select>
							</div>

							{#if source == 'Others'}
								<label for="username" class="mt-7 block text-xl font-medium leading-6 text-gray-900"
									>Please type the source <span class="text-red-600">*</span></label
								>
								<div class="mt-2">
									<input
										required
										bind:value={otherSource}
										type="text"
										name="name"
										id="name"
										class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-red-600 xl:text-xl xl:leading-10"
									/>
								</div>{/if}
						</div>
					</div>
				</div>

				<img
					src={MainImg}
					alt="main-img"
					class="relative object-contain w-full rounded max-h-lvh"
				/>

				<div class="mt-6 flex items-center justify-end gap-x-6">
					<button
						type="submit"
						class="rounded-md bg-red-600 px-7 py-3 text-xl font-semibold text-white shadow-sm hover:bg-red-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-red-600"
						>Submit</button
					>
				</div>
			</div>
		</form>
	</div>
</section>

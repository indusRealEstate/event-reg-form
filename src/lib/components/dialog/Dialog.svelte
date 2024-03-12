<script lang="ts">
	import { onMount } from 'svelte';
	import axios from 'axios';
	onMount(async () => {
		await axios.post(
			'https://indusre.ae/reg-form-jaipur-api/add_client.php',
			JSON.stringify({
				uid: formJson.uid,
				name: formJson.name,
				email: formJson.email,
				contact_no: formJson.contactNo,
				source: formJson.source
			}),
			{
				headers: {
					'Content-Type': 'application/json'
				}
			}
		);
	});

	export let formJson: any;

	let showDialog = true;

	const downloadImage = async () => {
		const data = `BEGIN%3aVCARD%0d%0aVERSION%3a3.0%0d%0aFN%3bCHARSET%3dUTF-8%3aJaipur-${formJson.name}%0d%0aN%3bCHARSET%3dUTF-8%3aDoe%3bJohn%3b%3b%3b%0d%0aTEL%3bTYPE%3dHOME%2cVOICE%3a${formJson.contactNo}%0d%0aTEL%3bTYPE%3dWORK%2cVOICE%3a%0d%0aEMAIL%3a${formJson.email}%0d%0aORG%3bCHARSET%3dUTF-8%3a%0d%0aURL%3a${formJson.uid}%3a%2f%2f%0d%0aEND%3aVCARD`;
		const url = `https://api.qrserver.com/v1/create-qr-code/?size=300x300&data=${data}`;
		if (!url.trim()) {
			alert('Please provide a valid image URL.');
			return;
		}

		try {
			const response = await fetch(url);
			const blob = await response.blob();

			let filenameToUse = 'qr-code';
			if (!filenameToUse) {
				// If no file name provided, generate a random one
				filenameToUse = Math.random().toString(36).substring(7) + '.jpg';
			}

			// Create a temporary anchor element to trigger the download
			const a = document.createElement('a');
			a.href = window.URL.createObjectURL(blob);
			a.download = filenameToUse;
			document.body.appendChild(a);
			a.click();
			document.body.removeChild(a);
			// showDialog = false;
			window.location.replace('https://indusre.com');
		} catch (error) {
			console.error('Error downloading image:', error);
			alert('Error downloading image. Please try again later.');
		}
	};

	const emailQr = async () => {
		try {
			const data = `BEGIN%3aVCARD%0d%0aVERSION%3a3.0%0d%0aFN%3bCHARSET%3dUTF-8%3aJaipur-${formJson.name}%0d%0aN%3bCHARSET%3dUTF-8%3aDoe%3bJohn%3b%3b%3b%0d%0aTEL%3bTYPE%3dHOME%2cVOICE%3a${formJson.contactNo}%0d%0aTEL%3bTYPE%3dWORK%2cVOICE%3a%0d%0aEMAIL%3a${formJson.email}%0d%0aORG%3bCHARSET%3dUTF-8%3a%0d%0aURL%3a${formJson.uid}%3a%2f%2f%0d%0aEND%3aVCARD`;
			const url = `https://api.qrserver.com/v1/create-qr-code/?size=300x300&data=${data}`;
			const qrResponse = await fetch(url);
			const blob = await qrResponse.blob();

			const reader = new FileReader();

			reader.readAsDataURL(blob);

			reader.onload = async function () {
				const base64Data = reader.result;

				// Call the API with base64 encoded image
				const response = await fetch('https://indusre.ae/send_email.php', {
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({ emailTo: formJson.email, image: base64Data, name: formJson.name })
				});

				if (!response.ok) {
					throw new Error('Network response was not ok');
				}

				const responseData = await response.text(); // or response.json() if the response is JSON
				console.log(responseData);
				window.location.replace('https://indusre.com');
			};
		} catch (error) {
			console.error('There was a problem with the fetch operation:', error);
			alert('API call failed!'); // Example: Show an alert
		}
	};
</script>

{#if showDialog}
	<div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
		<!--
      Background backdrop, show/hide based on modal state.
  
      Entering: "ease-out duration-300"
        From: "opacity-0"
        To: "opacity-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100"
        To: "opacity-0"
    -->
		<div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>

		<div class="fixed inset-0 z-10 w-screen overflow-y-auto">
			<div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
				<!--
          Modal panel, show/hide based on modal state.
  
          Entering: "ease-out duration-300"
            From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            To: "opacity-100 translate-y-0 sm:scale-100"
          Leaving: "ease-in duration-200"
            From: "opacity-100 translate-y-0 sm:scale-100"
            To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        -->
				<div
					class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg"
				>
					<div class="bg-white px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
						<div class="sm:flex sm:items-start">
							<!-- <div
							class="mx-auto flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10"
						>
							<svg
								class="h-6 w-6 text-red-600"
								fill="none"
								viewBox="0 0 24 24"
								stroke-width="1.5"
								stroke="currentColor"
								aria-hidden="true"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									d="M12 9v3.75m-9.303 3.376c-.866 1.5.217 3.374 1.948 3.374h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 3.378c-.866-1.5-3.032-1.5-3.898 0L2.697 16.126zM12 15.75h.007v.008H12v-.008z"
								/>
							</svg>
						</div> -->
							<div class="mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left">
								<h3 class="text-2xl font-semibold leading-6 text-gray-900" id="modal-title">
									Get Qr code
								</h3>
								<div class="mt-2">
									<p class="text-xl text-gray-500">
										Don't forget to bring the qr code with you when attending the event. This will
										help us verify your attendance. Please choose a way to collect the code.
									</p>
								</div>
							</div>
						</div>
					</div>
					<div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
						<button
							on:click={emailQr}
							type="button"
							class="inline-flex w-full justify-center rounded-md bg-red-600 px-3 py-2 text-xl font-semibold text-white shadow-sm hover:bg-red-500 sm:ml-3 sm:w-auto"
							>Via Email</button
						>
						<button
							on:click={downloadImage}
							type="button"
							class="inline-flex w-full justify-center rounded-md bg-red-600 px-3 py-2 text-xl font-semibold text-white shadow-sm hover:bg-red-500 sm:ml-3 sm:w-auto"
							>Download Now</button
						>
						<!-- <button
						type="button"
						class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto"
						>Cancel</button
					> -->
					</div>
				</div>
			</div>
		</div>
	</div>
{/if}

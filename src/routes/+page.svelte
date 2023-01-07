<script lang="ts">
	import type { PageData, ActionData } from './$types';
	import { enhance } from '$app/forms';
	import { PUBLIC_SERVER_ADDR } from '$env/static/public';

	let starting_imagedata;
	let error;
	let res_image;

	function onFileSelected(e: Event) {
		const file = e.target.files[0];
		const reader = new FileReader();
		reader.onload = (e) => {
			starting_imagedata = e.target.result;
			console.log('reader loaded');
		};
		reader.readAsDataURL(file);
	}
</script>

<form
	id="upload_image"
	class="flex flex-col gap-3"
	method="POST"
	use:enhance={async ({ form, data, action, cancel }) => {
		// `form` is the `<form>` element
		// `data` is its `FormData` object
		// `action` is the URL to which the form is posted
		// `cancel()` will prevent the submission
		const image = data.get('image');
		console.log('form loaded');
    res_image = undefined;

		const server_url = new URL(PUBLIC_SERVER_ADDR);
		server_url.pathname = '/maskImage';

		console.log('Sending request to server: ' + server_url.toString());

		const formData = new FormData();
		formData.append('image', image);

		try {
			const res = await fetch(server_url, {
				method: 'POST',
				body: formData
			});

			const buffer = await res.json();

			if (!res.ok) {
				error = { backend_error: true };
			}

			res_image = { image: 'data:image/png;base64,' + buffer['status'].split("'")[1] };
      error = undefined;
		} catch (err) {
      error = { backend_error: true };
			console.error(err);
		}

    cancel();

		return async ({ result, update }) => {
			// `result` is an `ActionResult` object
			// `update` is a function which triggers the logic that would be triggered if this callback wasn't set
		};
	}}
>
	{#if error?.backend_error}<p class="error">Error with backend, please try again. You may need to use a different image.</p>{/if}

	<div class="form-control w-full items-start">
		<label class="px-3 input-group-lg" for="image">
			<span>Upload an Image</span>
		</label>
		<input
			name="image"
			type="file"
			class="file-input file-input-bordered file-input-primary w-full"
			accept=".jpg, .jpeg, .png"
			on:change={(e) => onFileSelected(e)}
			required
		/>
	</div>
	{#if starting_imagedata !== undefined}
		<img class="m-auto w-60" src={starting_imagedata} alt="user uploaded" />
	{:else}
		<img class="m-auto w-60" src="/images/placeholder.png" alt="placeholder" />
	{/if}

	{#if res_image !== undefined && error === undefined}
		<a download="masked.png" href={res_image.image}>
			<img class="m-auto w-60" src={res_image.image} alt="user uploaded" />
		</a>
    <p>^ click me to download!</p>
	{/if}
	<button class="btn btn-primary w-full" type="submit" form="upload_image">Dis Nuts!</button>
</form>

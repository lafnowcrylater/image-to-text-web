<script lang="ts">
	import Tesseract from 'tesseract.js';

	let imageFile: File | undefined;
	let output: string = '';
	let status: string = '';
	let isProcessing: boolean = false;

	let previewUrl = '';

	function handleFileChange(e: Event & { currentTarget: HTMLInputElement }) {
		const input = e.currentTarget; // typed as HTMLInputElement
		imageFile = input.files?.[0];
		if (previewUrl) URL.revokeObjectURL(previewUrl); // clean up
		previewUrl = imageFile ? URL.createObjectURL(imageFile) : '';
	}

	async function processImage() {
		if (!imageFile) {
			alert('Please select an image first!');
			return;
		}

		isProcessing = true;
		status = 'Processing…';
		output = '';

		try {
			const result = await Tesseract.recognize(imageFile, 'eng');
			output = result.data.text;
			status = 'Done!';
		} catch (error) {
			console.error(error);
			status = 'Something went wrong.';
		} finally {
			isProcessing = false;
		}
	}
</script>

<svelte:head>
	<!-- <script src="https://cdn.jsdelivr.net/npm/tesseract.js@5/dist/tesseract.min.js"></script> -->
</svelte:head>

<main>
	<div class="card">
		<h2>Image to Text</h2>
		<p class="subtitle">Upload an image to extract its text.</p>

		<label class="file-label">
			<input type="file" accept="image/png, image/jpeg, image/webp" on:change={handleFileChange} />
			<span class="file-btn">
				{imageFile ? imageFile.name : 'Choose image…'}
			</span>
		</label>

		{#if previewUrl}
			<img src={previewUrl} alt="Preview" class="preview" />
		{/if}

		<button class="btn" on:click={processImage} disabled={isProcessing}>
			{isProcessing ? 'Extracting…' : 'Extract Text'}
		</button>

		{#if status}
			<p class="status" class:done={status === 'Done!'} class:error={status.includes('wrong')}>
				{status}
			</p>
		{/if}

		<textarea bind:value={output} placeholder="Extracted text will appear here…" readonly
		></textarea>
	</div>
</main>

<style>
	:global(body) {
		margin: 0;
		background: #f5f5f4;
		font-family: 'Georgia', serif;
	}

	main {
		min-height: 100vh;
		display: flex;
		align-items: flex-start;
		justify-content: center;
		padding: 60px 20px;
	}

	.card {
		background: #fff;
		border: 1px solid #e2e2e0;
		border-radius: 10px;
		padding: 40px;
		width: 100%;
		max-width: 560px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
	}

	h2 {
		margin: 0 0 6px;
		font-size: 1.5rem;
		font-weight: normal;
		color: #1a1a1a;
		letter-spacing: -0.3px;
	}

	.subtitle {
		margin: 0 0 28px;
		color: #888;
		font-size: 0.9rem;
	}

	.file-label {
		display: block;
		margin-bottom: 16px;
		cursor: pointer;
	}

	.file-label input[type='file'] {
		display: none;
	}

	.file-btn {
		display: block;
		padding: 10px 16px;
		border: 1.5px dashed #d0d0ce;
		border-radius: 6px;
		color: #666;
		font-size: 0.875rem;
		font-family: inherit;
		text-align: center;
		transition:
			border-color 0.15s,
			background 0.15s;
		background: #fafaf9;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.file-btn:hover {
		border-color: #999;
		background: #f0f0ee;
	}

	.btn {
		width: 100%;
		padding: 11px;
		background: #1a1a1a;
		color: #fff;
		border: none;
		border-radius: 6px;
		font-size: 0.9rem;
		font-family: inherit;
		cursor: pointer;
		transition: background 0.15s;
	}

	.btn:hover:not(:disabled) {
		background: #333;
	}

	.btn:disabled {
		background: #aaa;
		cursor: not-allowed;
	}

	.status {
		margin: 16px 0 0;
		font-size: 0.85rem;
		color: #888;
		text-align: center;
	}

	.status.done {
		color: #3a8a4a;
	}

	.status.error {
		color: #c0392b;
	}

	textarea {
		display: block;
		width: 100%;
		height: 200px;
		margin-top: 20px;
		padding: 12px;
		box-sizing: border-box;
		border: 1px solid #e2e2e0;
		border-radius: 6px;
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
		font-size: 0.85rem;
		color: #333;
		background: #fafaf9;
		resize: vertical;
		line-height: 1.5;
	}

	textarea:focus {
		outline: none;
		border-color: #aaa;
	}

	.preview {
		display: block;
		width: 100%;
		max-height: 260px;
		object-fit: contain;
		border: 1px solid #e2e2e0;
		border-radius: 6px;
		margin-bottom: 16px;
		background: #fafaf9;
	}
</style>

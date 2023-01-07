<script>
	import '../app.postcss';
</script>

<div class="hero min-h-screen bg-base-200">
	<div class="hero-content text-center">
		<div class="max-w-md">
			<h1 class="text-5xl font-bold">DIS Nuts!!</h1>
			<div>
				<p class="pt-6 pb-2">
					Use <a href="https://github.com/xuebinqin/DIS">DIS</a>, a really cool machine learning
					mode, to mask out the background of images.
				</p>
			</div>

			<div class="card w-96 bg-base-100 shadow-xl m-auto">
				<div class="card-body">
					<slot />
				</div>
			</div>
			<button class="pt-6 pb-4" data-toggle-theme="emerald,dracula">change theme</button>
			<p class="pb-2">
				Made with 🧡 by <a
					class="link hover:text-secondary transition-colors["
					href="https://github.com/ava-ankenbrandt">Ava</a
				>
				and
				<a class="link hover:text-secondary transition-colors[" href="https://cobular.com"
					>Cobular</a
				>
			</p>
		</div>
	</div>
</div>

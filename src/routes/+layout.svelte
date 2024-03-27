<script lang="ts">
	import '../app.postcss';
	import '@flaticon/flaticon-uicons/css/all/all.css';
	import { AppShell, AppBar } from '@skeletonlabs/skeleton';
	// Highlight JS
	import hljs from 'highlight.js/lib/core';
	import 'highlight.js/styles/github-dark.css';
	import { storeHighlightJs } from '@skeletonlabs/skeleton';
	import xml from 'highlight.js/lib/languages/xml'; // for HTML
	import css from 'highlight.js/lib/languages/css';
	import java from 'highlight.js/lib/languages/java';
	import cpp from 'highlight.js/lib/languages/cpp';
	import python from 'highlight.js/lib/languages/python';
	import javascript from 'highlight.js/lib/languages/javascript';
	import typescript from 'highlight.js/lib/languages/typescript';

	hljs.registerLanguage('xml', xml); // for HTML
	hljs.registerLanguage('css', css);
	hljs.registerLanguage('javascript', javascript);
	hljs.registerLanguage('typescript', typescript);
	hljs.registerLanguage('java', java);
	hljs.registerLanguage('cpp', cpp);
	hljs.registerLanguage('python', python);
	storeHighlightJs.set(hljs);

	// Floating UI for Popups
	import { computePosition, autoUpdate, flip, shift, offset, arrow } from '@floating-ui/dom';
	import { storePopup } from '@skeletonlabs/skeleton';
	import { base } from '$app/paths';
	import { goto } from '$app/navigation';
	storePopup.set({ computePosition, autoUpdate, flip, shift, offset, arrow });
</script>

<!-- App Shell -->
<AppShell>
	<svelte:fragment slot="header">
		<!-- App Bar -->
		<AppBar>
			<svelte:fragment slot="lead">
				<button class="text-xl uppercase" on:click={()=>{goto(`${base}/`)}} on:keydown={(event) => {if (event.key === 'Enter') goto(`${base}/`);}}>ACSL Club</button>
			</svelte:fragment>
			<svelte:fragment slot="trail">
				<a
					class="btn btn-sm variant-ghost-surface"
					href="https://discord.gg/swT6yFGnYb"
					target="_blank"
					rel="noreferrer"
				>
					Discord
				</a>
				<a
					class="btn btn-sm variant-ghost-surface"
					href="{base}/signup"
					target="_blank"
					rel="noreferrer"
				>
					Sign up
				</a>
				<!-- <a
					class="btn btn-sm variant-ghost-surface"
					href="https://github.com/skeletonlabs/skeleton"
					target="_blank"
					rel="noreferrer"
				>
					GitHub
				</a> -->
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<!-- Page Route Content -->
	<slot />
</AppShell>

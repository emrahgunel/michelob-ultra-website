
<script context="module" >
	export const prerender = true;
</script>

<script >
	import Supdawg from '$lib/Supdawg.svelte';
	import Banner from '$lib/Banner.svelte';
	import { page } from '$app/stores';
	import * as BuilderSDK from '@builder.io/sdk-svelte';
	// TODO: enter your public API key
	//const BUILDER_PUBLIC_API_KEY = '87eb56a463224e9abd7f09701429c87d';  // ggignore
	const BUILDER_PUBLIC_API_KEY = '1535c91332ea4270b0feb462e9308afe'; // Emrah's Test Space 
	let content = undefined;
	let canShowContent = false;
	const fetch = async () => {
		content = await BuilderSDK.getContent({
			model: 'page',
			apiKey: BUILDER_PUBLIC_API_KEY,
			//options: BuilderSDK.getBuilderSearchParams($page.params),
			options: BuilderSDK.getBuilderSearchParams(BuilderSDK.convertSearchParamsToQueryObject($page.url.searchParams)),
			userAttributes: {
				urlPath: $page.url.pathname
			}
		});
		canShowContent = content || BuilderSDK.isEditing();
	};

	BuilderSDK.registerComponent(Supdawg, 
		{ 	
			name: "Supdawg",
			inputs: 
				[
					{
						name: "inputText",
						type: "string",
						defaultValue: "What is Supdawg?",
					},
					{
						name: "language",
						type: "string",
						defaultValue: "javascript",
					}
				]
		});

	BuilderSDK.registerComponent(Banner, 
		{ 	
			name: "Ultra Banner",
			inputs: 
				[
					{
						name: "bannerTitle",
						type: "string",
						defaultValue: "SAVE IT, SEE IT",
					},
					{
						name: "text",
						type: "string",
						defaultValue: "WHEN WE HIT SAVE ON WOMEN'S SPORT",
					},
					{
						name: "buttonUrl",
						type: "url",
					},
					{ 
						name: 'image',
						type: 'file', 
						allowedFileTypes: ['jpeg', 'png'] 
  }
				]
		});
	fetch();

</script>

<h5>This Layout is from the Index.Svelte file, since the relative url is '/' <br /> 
	{'The title of this page in builder: ' + (content && content.data && content.data.title) || 'This page either does not exist or is currently unpublished'}
</h5>

<section class="builderZone">
	<BuilderSDK.RenderContent model="page" {content} api-key={BUILDER_PUBLIC_API_KEY} />
</section>


<style>
	.builderZone {
		padding: 40px;
		background-color: rgba(0,0,0, 0.05);
	}
</style>

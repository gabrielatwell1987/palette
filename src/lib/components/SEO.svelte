<script>
	import { page } from '$app/stores';

	/** @type {{title: string, description: string, keywords: string, image?: string, type?: string}} */
	let { keywords, description, title, image, siteType = 'website' } = $props();
	let url = $derived($page.url.href);
	let siteName = 'PixiPalette';
	let baseUrl = $derived($page.url.origin);
	let ogImage = $derived(image ? `${baseUrl}${image}` : `${baseUrl}/logos/pixipalette.webp`);
</script>

<svelte:head>
	<title>{title}</title>

	<meta name="description" content={description} />
	<meta name="keywords" content={keywords} />
	<meta charset="utf-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1" />

	<link rel="canonical" href={url} />

	<meta name="robots" content="index, follow" />
	<meta property="og:locale" content="en_US" />

	<meta property="og:title" content={title} />
	<meta property="og:description" content={description} />
	<meta property="og:url" content={url} />
	<meta property="og:type" content={siteType} />
	<meta property="og:site_name" content={siteName} />
	<meta property="og:image" content={ogImage} />
	<meta property="og:image:alt" content={`${title} - ${siteName}`} />
	<meta property="og:image:width" content="1200" />
	<meta property="og:image:height" content="630" />

	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content={title} />
	<meta name="twitter:description" content={description} />
	<meta name="twitter:image" content={ogImage} />
	<meta name="twitter:image:alt" content={`${title} - ${siteName}`} />

	<script type="application/ld+json">
        {JSON.stringify({
            "@context": "https://schema.org",
            "@type": "WebSite",
            "name": siteName,
            "description": description,
            "url": baseUrl,
            "potentialAction": {
                "@type": "SearchAction",
                "target": {
                    "@type": "EntryPoint",
                    "urlTemplate": `${baseUrl}/?q={search_term_string}`
                },
                "query-input": "required name=search_term_string"
            }
        })}
	</script>
</svelte:head>

<script>
    import "../app.css";
    import Header from '$lib/components/Header.svelte'; 
    let { children } = $props();
</script>

<svelte:head>
    <link rel="icon" href="/favicon/favicon.ico" />
    <link rel="icon" type="image/png" href="/favicon/favicon-32x32.png" sizes="32x32" />
    <link rel="icon" type="image/png" href="/favicon/favicon-16x16.png" sizes="16x16" />

    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />

    <link
        rel="preload"
        as="style"
        href="https://fonts.googleapis.com/css2?family=DM+Serif+Text:ital@0;1&family=Mona+Sans:ital,wght@0,200..900;1,200..900&display=swap"
    />

    <link
        rel="stylesheet"
        href="https://fonts.googleapis.com/css2?family=DM+Serif+Text:ital@0;1&family=Mona+Sans:ital,wght@0,200..900;1,200..900&display=swap"
        media="print"
        on:load={(e) => (e.target.media = 'all')}
    />

    <noscript>
        <link
            rel="stylesheet"
            href="https://fonts.googleapis.com/css2?family=DM+Serif+Text:ital@0;1&family=Mona+Sans:ital,wght@0,200..900;1,200..900&display=swap"
        />
    </noscript>
    <title>IKU - APP</title>
</svelte:head>


<Header />

<main class="flex flex-col min-h-screen">
    <div class="grow">
        {@render children?.()}
    </div>
</main>

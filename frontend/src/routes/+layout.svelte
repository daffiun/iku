<script>
    import "../app.css";
    import Header from '$lib/components/Header.svelte'; 
<<<<<<< HEAD
=======
    import About from '$lib/components/About.svelte';
	import Team from '$lib/components/Team.svelte';
    import Stack from '$lib/components/Stack.svelte';
    import Faq from '$lib/components/Faq.svelte';
    import Footer from '$lib/components/Footer.svelte';
>>>>>>> 14ed3b2ee226ea8c16a9568b1b0e5c07ee747aa8
    let { children } = $props();
</script>

<svelte:head>
    <link rel="icon" href="/favicon/favicon.ico" />
    <link rel="icon" type="image/png" href="/favicon/favicon-32x32.png" sizes="32x32" />
    <link rel="icon" type="image/png" href="/favicon/favicon-16x16.png" sizes="16x16" />
</svelte:head>

<Header />
<<<<<<< HEAD
=======
<About/>
<Team/>
<Faq />
<Contact />
<Footer />

>>>>>>> 14ed3b2ee226ea8c16a9568b1b0e5c07ee747aa8

<main class="flex flex-col min-h-screen">
    <div class="grow">
        {@render children?.()}
    </div>
</main>

<script>
  export const prerender = true;
  export const ssr = true;

  export const trailingSlash = 'ignore';
  export const load = async () => {};
</script>

<svelte:head>
  <title>Custom Layout</title>
</svelte:head>

<slot />






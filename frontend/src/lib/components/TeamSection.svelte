
<script lang="ts">
  /**
   * Minimal, type-safe SvelteKit component for a team section.
   * Accepts `members` array and renders each member responsively.
   * Uses Tailwind classes for styling; if you don't use Tailwind remove/replace classes.
   */

  export interface Socials {
    twitter?: string;
    github?: string;
    linkedin?: string;
    website?: string;
    [key: string]: string | undefined;
  }

  export interface Member {
    id: string | number;
    name: string;
    role?: string;
    photo?: string; // relative or absolute URL; if empty, initials avatar shown
    bio?: string;
    socials?: Socials;
  }

  export let members: Member[] = [];
  export let heading: string = 'Our Team';
  export let description: string = '';

  
  // helper: initials from full name
  function getInitials(name: string) {
    if (!name) return '';
    const parts = name.trim().split(/\s+/);
    const initials = parts.slice(0, 2).map(p => p[0]?.toUpperCase() ?? '').join('');
    return initials;
  }

  // helper: small inline SVG icons by key (github, twitter, linkedin, website)
  function iconFor(key: string) {
    switch (key.toLowerCase()) {
      case 'github':
        return `<!-- github -->
          <svg xmlns=\"http://www.w3.org/2000/svg\" width=\"20\" height=\"20\" viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"1.5\" stroke-linecap=\"round\" stroke-linejoin=\"round\"><path d=\"M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 10.5 18V22\"/></svg>`;
      case 'twitter':
        return `<!-- twitter -->
          <svg xmlns=\"http://www.w3.org/2000/svg\" width=\"20\" height=\"20\" viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"1.5\" stroke-linecap=\"round\" stroke-linejoin=\"round\"><path d=\"M23 3a10.9 10.9 0 0 1-3.14 1.53A4.48 4.48 0 0 0 22.43.36a9.05 9.05 0 0 1-2.88 1.1A4.5 4.5 0 0 0 16 0c-2.48 0-4.5 2.24-4.5 5 0 .39.05.77.13 1.13A12.94 12.94 0 0 1 1.64 1.16a5 5 0 0 0-.61 2.52A4.9 4.9 0 0 0 3.3 7.8 4.48 4.48 0 0 1 .9 7.4v.06A4.93 4.93 0 0 0 4.5 12a4.55 4.55 0 0 1-2 .08 4.51 4.51 0 0 0 4.2 3.13A9 9 0 0 1 0 18.54 12.7 12.7 0 0 0 6.92 20c8.31 0 12.86-6.93 12.86-12.94 0-.2 0-.4-.02-.6A9.22 9.22 0 0 0 23 3z\"/></svg>`;
      case 'linkedin':
        return `<!-- linkedin -->
          <svg xmlns=\"http://www.w3.org/2000/svg\" width=\"20\" height=\"20\" viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"1.5\" stroke-linecap=\"round\" stroke-linejoin=\"round\"><path d=\"M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2c-1.5 0-2 1-2 2v7h-4v-14h4v2a4 4 0 0 1 4-2zM2 9h4v14H2zM4 3a2 2 0 1 1 0 4 2 2 0 0 1 0-4z\"/></svg>`;
      default:
        return `<!-- link -->
          <svg xmlns=\"http://www.w3.org/2000/svg\" width=\"20\" height=\"20\" viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"1.5\" stroke-linecap=\"round\" stroke-linejoin=\"round\"><path d=\"M10 13a5 5 0 0 0 7.07 0l3-3a5 5 0 0 0-7.07-7.07l-1.4 1.4\"/><path d=\"M14 11a5 5 0 0 0-7.07 0l-3 3a5 5 0 0 0 7.07 7.07L12.4 19\"/></svg>`;
    }
  }
</script>

<section aria-labelledby="team-heading" class="py-12">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="text-center mb-8">
      <h2 id="team-heading" class="text-3xl font-extrabold tracking-tight sm:text-4xl">{heading}</h2>
      {#if description}
        <p class="mt-2 text-lg text-gray-600">{description}</p>
      {/if}
    </div>

    {#if members && members.length > 0}
      <ul role="list" class="grid gap-8 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
        {#each members as member (member.id)}
          <li class="bg-white/80 dark:bg-slate-800/70 p-6 rounded-2xl shadow-sm flex flex-col items-center text-center">
            {#if member.photo}
              <img src={member.photo} alt={member.name} class="w-28 h-28 rounded-full object-cover mb-4" loading="lazy" />
            {:else}
              <div class="w-28 h-28 rounded-full bg-gray-200 dark:bg-slate-700 mb-4 flex items-center justify-center text-2xl font-semibold text-gray-700 dark:text-gray-100">
                {getInitials(member.name)}
              </div>
            {/if}

            <div class="flex-1">
              <h3 class="text-lg font-semibold">{member.name}</h3>
              {#if member.role}
                <p class="text-sm text-indigo-600 dark:text-indigo-400">{member.role}</p>
              {/if}
              {#if member.bio}
                <p class="mt-3 text-sm text-gray-600 dark:text-gray-300">{member.bio}</p>
              {/if}
            </div>

            {#if member.socials}
              <div class="mt-4 flex gap-3">
                {#each Object.entries(member.socials) as [key, url]}
                  {#if url}
                    <a href={url} target="_blank" rel="noopener noreferrer" aria-label={`${member.name} on ${key}`} class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-slate-700">
                      {@html iconFor(key)}
                    </a>
                  {/if}
                {/each}
              </div>
            {/if}
          </li>
        {/each}
      </ul>
    {:else}
      <div class="text-center text-gray-500">No team members to display.</div>
    {/if}
  </div>
</section>


<style>
  /* If you are *not* using Tailwind, these are lightweight fallbacks to keep layout usable. Remove when using Tailwind. */
  :global(body) { --card-bg: #fff; }
  section { font-family: system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; }
  @media (min-width: 640px) {
    ul { display: grid; grid-template-columns: repeat(2, minmax(0,1fr)); }
  }
  @media (min-width: 1024px) {
    ul { grid-template-columns: repeat(3, minmax(0,1fr)); }
  }
</style>


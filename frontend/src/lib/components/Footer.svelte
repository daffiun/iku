<script>
  const year = new Date().getFullYear();

  const footerLinks = [
    { text: "Tentang Kami", href: "#about" },
    { text: "Fitur", href: "#fitur" },
    { text: "Tim", href: "#ourteam" },
    { text: "FAQ", href: "#faq" },
    { text: "Kontak", href: "#contact_us" }
  ];

  const socials = [
    { icon: "github", href: "https://github.com/daffiun/iku" },
    { icon: "linkedin", href: "#" },
    { icon: "twitter", href: "#" }
  ];

  function icon(name) {
    switch (name) {
      case "github":
        return `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1 13.38 13.38 0 0 0 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1 5.09 1 5 4.77 5 4.77c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 10.5 18V22"/></svg>`;
      case "linkedin":
        return `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7c0-1-.5-2-2-2s-2 1-2 2v7h-4v-14h4v2c1-1.5 2.5-2 4-2zM2 9h4v14H2z"/><circle cx="4" cy="4" r="2"/></svg>`;
      case "twitter":
        return `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M23 3a10.9 10.9 0 0 1-3.14 1.53A4.48 4.48 0 0 0 22.43.36 8.8 8.8 0 0 1 19.55 1.46 4.48 4.48 0 0 0 16 0c-2.48 0-4.5 2.24-4.5 5 0 .39.05.77.13 1.13A12.94 12.94 0 0 1 1.64 1.16 5 5 0 0 0 1 3.68 4.9 4.9 0 0 0 3.3 7.8 4.48 4.48 0 0 1 .9 7.4v.06A4.93 4.93 0 0 0 4.5 12a4.55 4.55 0 0 1-2 .08 4.5 4.5 0 0 0 4.2 3.15A9.05 9.05 0 0 1 0 18.5 12.77 12.77 0 0 0 6.9 20c8.31 0 12.86-6.9 12.86-13 0-.2 0-.4-.02-.6A9.22 9.22 0 0 0 23 3z"/></svg>`;
    }
  }
</script>

<footer class="w-full bg-black text-gray-300 pt-24 pb-8 mt-20 border-t border-white/10">
  <div class="max-w-7xl mx-auto px-6 flex flex-col md:flex-row md:justify-between gap-12">

    <div class="space-y-4 relative">
      <img src="/images/logos/logo.svg" width="250" class="absolute" style="top: -100px;" alt="" srcset="">
      <h3 class="text-white text-xl font-semibold tracking-wide" style="margin-top: 115px;">IKU — Index Kewirausahaan UMKM</h3>
      <p class="max-w-sm text-gray-400 text-sm">
        Mendorong UMKM Indonesia melalui data, inovasi, dan analisis yang lebih baik.
      </p>

      <div class="flex gap-4 pt-3">
        {#each socials as item}
          <a
            href={item.href}
            class="p-2 rounded-full bg-white/10 hover:bg-orange-500 transition-colors duration-200"
            aria-label={item.icon}
          >
            {@html icon(item.icon)}
          </a>
        {/each}
      </div>
    </div>

    <div class="grid grid-cols-2 sm:grid-cols-3 gap-8 text-sm">
      <div class="space-y-3">
        <h4 class="text-white font-semibold">Navigasi</h4>
        {#each footerLinks as link}
          <a href={link.href} class="block hover:text-orange-500 transition-colors">{link.text}</a>
        {/each}
      </div>

      <div class="space-y-3">
        <h4 class="text-white font-semibold">Lainnya</h4>
        <a href="/umkm" class="block hover:text-orange-500">Jelajahi UMKM</a>
        <a href="/maps" class="block hover:text-orange-500">Peta UMKM</a>
      </div>

      <div class="space-y-3">
        <h4 class="text-white font-semibold">Location</h4>
        <p class="text-gray-400">Surabaya, Indonesia</p>
      </div>
    </div>
  </div>

  <!-- Bottom -->
  <div class="text-center mt-12 pt-8 border-t border-white/10 text-gray-500 text-sm">
    © {year} IKU — All rights reserved.
  </div>
</footer>

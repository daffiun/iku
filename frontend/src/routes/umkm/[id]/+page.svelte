<script>
    import { onMount } from "svelte";
    import { page } from "$app/stores";
    import { gsap } from "gsap";
    import { ScrollTrigger } from "gsap/dist/ScrollTrigger";

    import data from "$lib/assets/dummy/umkm.json";

    gsap.registerPlugin(ScrollTrigger);

    $: id = parseInt($page.params.id);
    $: umkm = data.find((item) => item.id === id);

    let container;

    onMount(() => {
        if (!umkm) return;

        gsap.from(".fade", {
            opacity: 0,
            y: 40,
            duration: 0.9,
            ease: "power3.out",
            stagger: 0.15,
            delay: 0.2
        });
    });
</script>

<section class="pt-24 bg-neutral-950 text-white min-h-screen">

    {#if !umkm}
        <div class="text-center mt-24 text-xl font-bold text-orange-500 tracking-wide">
            UMKM tidak ditemukan.
        </div>
    {:else}

        <div bind:this={container} class="max-w-5xl mx-auto px-4">

            <header
                class="fade bg-orange-600 py-6 text-center text-4xl font-black rounded-sm
                shadow-[0_0_40px_rgba(255,100,0,0.5)] tracking-wide border border-orange-700"
            >
                {umkm.nama}
            </header>

            <div class="gallery fade flex overflow-x-auto mt-6 rounded-sm space-x-3 snap-x snap-mandatory">
                {#each umkm.image as img}
                    <img
                        src={img}
                        alt={umkm.nama}
                        class="w-full h-[380px] object-cover rounded-lg flex-shrink-0 snap-center
                        shadow-[0_0_25px_rgba(0,0,0,0.7)] border border-neutral-800"
                    />
                {/each}
            </div>

            <div class="content fade mt-10 space-y-8 
           bg-neutral-900/40 backdrop-blur-sm
           p-8 md:p-10 rounded-md border border-neutral-800
           shadow-[0_0_35px_rgba(0,0,0,0.7)]">

                <h2 class="fade text-3xl md:text-4xl font-black text-orange-500 tracking-tight">
                    {umkm.nama}
                </h2>

                <p class="fade text-lg italic text-neutral-300 font-medium">
                    {umkm.short_desc}
                </p>

                <div class="fade text-neutral-200 leading-relaxed text-lg">
                    {umkm.desc}
                </div>

                <div
                    class="fade bg-neutral-900 px-6 py-5 border-l-4 border-orange-600 rounded-lg
                    shadow-[0_0_20px_rgba(255,120,0,0.25)] space-y-2 text-lg font-medium"
                >
                    <p><strong class="text-orange-500">Alamat:</strong> {umkm.alamat}</p>
                    <p><strong class="text-orange-500">Jam Buka:</strong> {umkm.waktu_buka} – {umkm.waktu_tutup}</p>
                    <p><strong class="text-orange-500">Koordinat:</strong> {umkm.coor.lat}, {umkm.coor.lng}</p>
                </div>

                <div class="fade flex flex-wrap gap-4">
                    {#each Object.entries(umkm.social_media) as [key, value]}
                        <a
                            href={"https://" + (value.includes("http") ? value : (key === "x"
                                ? "x.com/"
                                : key + ".com/") + value.replace("@", ""))}
                            target="_blank"
                            rel="noopener noreferrer"
                            class="bg-neutral-900 px-4 py-2 rounded-lg font-bold text-orange-500
                            hover:bg-neutral-800 transition shadow border border-neutral-700
                            hover:shadow-[0_0_12px_rgba(255,120,0,0.4)]"
                        >
                            {key.toUpperCase()}
                        </a>
                    {/each}
                </div>

                <div class="fade">
                    <h3 class="text-2xl font-black text-orange-500 mb-4 tracking-tight">
                        Produk
                    </h3>

                    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-5">
                        {#each umkm.produk as p}
                            <div class="bg-neutral-900 rounded-sm p-4 text-center shadow
                                hover:shadow-[0_0_18px_rgba(255,120,0,0.4)] hover:-translate-y-1
                                transition border border-neutral-800"
                            >
                                <img
                                    src={p.img}
                                    alt={p.nama}
                                    class="w-full h-32 object-cover rounded-sm mb-3 border border-neutral-700"
                                />
                                <p class="font-bold tracking-wide">{p.nama}</p>
                            </div>
                        {/each}
                    </div>
                </div>

            </div>
        </div>
    {/if}

</section>


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

<section class="page">

    <!-- If not found -->
    {#if !umkm}
        <div class="not-found">
            <p>UMKM tidak ditemukan.</p>
        </div>
    {:else}

        <div bind:this={container} class="wrapper">

            <!-- Title Banner -->
            <header class="banner fade">
                <h1>{umkm.nama}</h1>
            </header>

            <!-- Gallery -->
            <div class="gallery fade">
                {#each umkm.image as img}
                    <img src={img} alt={umkm.nama} />
                {/each}
            </div>

            <!-- Content -->
            <div class="content">

                <h2 class="fade">{umkm.nama}</h2>
                <p class="short-desc fade">{umkm.short_desc}</p>

                <div class="desc fade">{umkm.desc}</div>

                <div class="info fade">
                    <p><strong>Alamat:</strong> {umkm.alamat}</p>
                    <p><strong>Jam Buka:</strong> {umkm.waktu_buka} – {umkm.waktu_tutup}</p>
                    <p><strong>Koordinat:</strong> {umkm.coor.lat}, {umkm.coor.lng}</p>
                </div>

                <!-- Social media -->
                <div class="social-media fade">
                    {#each Object.entries(umkm.social_media) as [key, value]}
                        <a
                            href={"https://" + (value.includes("http") ? value : (key === "x"
                                ? "x.com/" 
                                : key + ".com/") + value.replace("@", ""))}
                            target="_blank"
                            rel="noopener noreferrer"
                        >
                            {key.toUpperCase()}
                        </a>
                    {/each}
                </div>

                <!-- Produk -->
                <div class="produk fade">
                    <h3>Produk</h3>
                    <div class="produk-list">
                        {#each umkm.produk as p}
                            <div class="produk-item">
                                <img src={p.img} alt={p.nama} />
                                <p>{p.nama}</p>
                            </div>
                        {/each}
                    </div>
                </div>
            </div>
        </div>
    {/if}

</section>

<style>
.page {
    padding-top: 5rem;
}

:global(body) {
    background: #111;
    color: #fff;
    font-family: "Poppins", sans-serif;
}

/* Not found */
.not-found {
    text-align: center;
    margin-top: 6rem;
    font-size: 1.3rem;
    color: #ff8800;
}

/* Wrapper */
.wrapper {
    max-width: 1050px;
    margin: 2rem auto;
    padding: 0 1rem;
}

/* Header Banner */
.banner {
    background: #ff6600;
    padding: 1.5rem;
    text-align: center;
    font-size: 2rem;
    font-weight: 800;
    border-radius: 8px;
    box-shadow: 0 0 18px rgba(255, 102, 0, 0.35);
    letter-spacing: 1px;
}

/* Gallery */
.gallery {
	opacity: 1;
    margin-top: 1.7rem;
    display: flex;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    border-radius: 10px;
}

.gallery img {
    width: 100%;
    height: 380px;
    object-fit: cover;
    flex-shrink: 0;
    scroll-snap-align: center;
    border-radius: 10px;
    margin-right: 10px;
	opacity : 1;
}

/* Content */
.content {
    margin-top: 2.5rem;
}

h2 {
    font-size: 2rem;
    font-weight: 800;
    color: #ff6600;
}

.short-desc {
    opacity: 0.75;
    font-style: italic;
    margin-top: 6px;
}

.desc {
    margin: 1.5rem 0;
    line-height: 1.65;
    font-size: 1.05rem;
}

/* Info Box */
.info {
    background: #222;
    padding: 1.2rem 1.4rem;
    border-left: 4px solid #ff6600;
    border-radius: 8px;
    margin-bottom: 1.5rem;
}

.info strong {
    color: #ff6600;
}

/* Social media */
.social-media {
    margin-bottom: 2rem;
}

.social-media a {
    color: #ff6600;
    font-weight: 600;
    margin-right: 1rem;
    text-decoration: none;
    transition: 0.2s;
}
.social-media a:hover {
    opacity: 0.7;
}

/* Produk */
.produk h3 {
    font-size: 1.7rem;
    color: #ff6600;
    font-weight: 700;
    margin-bottom: 1rem;
}

.produk-list {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 1.2rem;
}

.produk-item {
    background: #1f1f1f;
    padding: 1rem;
    border-radius: 10px;
    text-align: center;
    transition: 0.25s;
    border: 1px solid #2a2a2a;
}

.produk-item:hover {
    transform: translateY(-4px);
    box-shadow: 0 0 12px rgba(255, 102, 0, 0.25);
}

.produk-item img {
    width: 100%;
    height: 130px;
    object-fit: cover;
    border-radius: 8px;
}

.produk-item p {
    margin-top: 0.7rem;
    font-weight: 600;
}

/* Responsive */
@media (max-width: 650px) {
    .gallery img {
        height: 250px;
    }
    .banner {
        font-size: 1.6rem;
        padding: 1.2rem;
    }
}
</style>


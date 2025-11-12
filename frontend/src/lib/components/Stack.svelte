<script>
  import { onMount } from "svelte";
  import { gsap } from "gsap";

  let slider1;
  let slider2;
  let animation1, animation2;
  let animationSpeed = 1;
  let targetSpeed = 1;
  let lastScrollY = 0;
  let scrollVelocity = 0;

  const logos = [
    { name: "svelte", img: "/logos/svelte.svg" },
    { name: "js", img: "/logos/js.png" },
    { name: "tailwind", img: "/logos/tailwind.svg" },
    { name: "vite", img: "/logos/vite.png" },
    { name: "gsap", img: "/logos/gsap.svg" },
  ];

  onMount(() => { 
    // --- SETUP SLIDER WIDTH
    const totalWidth = slider1.scrollWidth / 2;

    // --- SLIDER 1 (kiri)
    animation1 = gsap.to(slider1, {
      x: `-=${totalWidth}`,
      duration: 40,
      ease: "none",
      repeat: -1,
      modifiers: {
        x: gsap.utils.unitize((x) => parseFloat(x) % totalWidth),
      },
    });

    // --- SLIDER 2 (kanan, arah berlawanan)
    animation2 = gsap.to(slider2, {
      x: `+=${totalWidth}`,
      duration: 40,
      ease: "none",
      repeat: -1,
      modifiers: {
        x: gsap.utils.unitize((x) => parseFloat(x) % totalWidth),
      },
    });

    window.addEventListener("scroll", handleScroll, { passive: true });
    smoothBack();

    return () => {
      window.removeEventListener("scroll", handleScroll);
      animation1.kill();
      animation2.kill();
    };
  });
</script>

<style>
  section {
    background: black;
    color: white;
  }
</style>

<section class="py-20 flex flex-col items-center gap-8 overflow-hidden relative">
  <h2 class="text-center text-gray-200 text-xl tracking-wide font-mono">
    Our Stack    
  </h2>

  <!-- SLIDER 1 -->
  <div class="relative overflow-hidden w-full">
    <div class="absolute inset-y-0 left-0 w-48 bg-linear-to-r from-black to-transparent z-10"></div>
    <div class="absolute inset-y-0 right-0 w-48 bg-linear-to-l from-black to-transparent z-10"></div>

    <div
      class="flex gap-8 will-change-transform whitespace-nowrap"
      bind:this={slider1}
    >
      {#each [...logos, ...logos, ...logos] as logo}
        <div class="min-w-[120px] flex justify-center items-center">
          <!-- Gambar jadi putih -->
          <img
            src={logo.img}
            alt={logo.name}
            class="w-20"
          />
        </div>
      {/each}
    </div>
  </div>
</section>


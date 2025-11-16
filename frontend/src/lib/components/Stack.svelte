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
    { name: "svelte", img: "/stack/svelte.svg" },
    { name: "js", img: "/stack/js.svg" },
    { name: "tailwind", img: "/stack/tailwind.svg" },
    { name: "vite", img: "/stack/vite.png" },
    { name: "gsap", img: "/stack/gsap.svg" },
  ];

  function handleScroll() {
    const currentY = window.scrollY;
    const delta = currentY - lastScrollY;

    const velocity = Math.min(Math.abs(delta) / 40, 3);
    targetSpeed = 1 + velocity;

    lastScrollY = currentY;
  }

  function smoothBack() {
    gsap.ticker.add(() => {
      animationSpeed += (targetSpeed - animationSpeed) * 0.06;
      animation1?.timeScale(animationSpeed);
      animation2?.timeScale(animationSpeed);

      targetSpeed += (1 - targetSpeed) * 0.04; 
    });
  }

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

<section class="py-16 flex flex-col items-center gap-6 overflow-hidden relative px-4">
  <h2 class="text-center text-gray-200 text-lg sm:text-xl tracking-wide font-mono">
    Our Stack
  </h2>

  <div class="relative overflow-hidden w-full">
    <div class="absolute inset-y-0 left-0 w-16 sm:w-48 bg-gradient-to-r from-black to-transparent z-10"></div>

    <div class="absolute inset-y-0 right-0 w-16 sm:w-48 bg-gradient-to-l from-black to-transparent z-10"></div>

    <div
      class="flex gap-6 sm:gap-8 will-change-transform whitespace-nowrap py-4"
      bind:this={slider1}
    >
      {#each [...logos, ...logos, ...logos] as logo}
        <div class="min-w-[90px] sm:min-w-[120px] flex justify-center items-center">
          <img src={logo.img} alt={logo.name} class="w-14 sm:w-20" />
        </div>
      {/each}
    </div>
  </div>
</section>


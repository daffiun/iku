<script>
  import { onMount } from "svelte";
  import gsap from "gsap";

  let sliderContainer;
  let animationSpeed = 1;
  let lastScrollY = 0;
  let scrollVelocity = 0;
  let animation;
  let lenis;

  let sliderContainer2;
  let animation2;

  const techStack = [
    { name: "vite", img: "vite.svg" },
    { name: "svelte", img: "svelte.svg" },
    { name: "tailwind", img: "tailwind.svg" },
    { name: "gsap", img: "gsap.svg" },
    { name: "javascript", img: "js.svg" },
  ];

  onMount(() => {
    // Setup infinite slider animation
    const totalWidth = sliderContainer.scrollWidth / 2;

    // Infinite loop
    animation = gsap.to(sliderContainer, {
      x: -totalWidth,
      duration: 30,
      ease: "none",
      repeat: -1,
      modifiers : {
        x: gsap.utils.unitize(x => parseFloat(x) % totalWidth)
      }
    });

    // scroll handler
    const handleScroll = () => {
      const currentScrollY = window.scrollY;
      scrollVelocity = currentScrollY -lastScrollY;
      lastScrollY = currentScrollY;

      // hitung kecepatan arah scroll
      if (scrollVelocity > 0) {
        //scroll kebawah -> slider lebih cepat ke kiri
        animationSpeed = 1 + (Math.abs(scrollVelocity) * 0.5);
      } else if(scrollVelocity < 0){
        // Scroll ke atas -> slider lebih cepat ke kanan
        animationSpeed = -1 - (Math.abs(scrollVelocity * 0.5));
      }

      animation.timeScale(animationSpeed);
    };

    const smoothSpeed = () => {
      //Kembalikan kecepatan normal bertahap
      animationSpeed += (1-animationSpeed) * 0.05;
      animation.timeScale(animationSpeed);

      requestAnimationFrame(smoothSpeed);
    };

    window.addEventListener('scroll', handleScroll, { passive: true});
    smoothSpeed();

    animation2 = gsap.to(sliderContainer2, {

    });

    return () => {
      window.removeEventListener('scroll', handleScroll);
      animation.kill();
    }
  });
</script>   
<section class="relative overflow-hidden ">
  <!-- Shadow kanan kiri -->
  <div class="pointer-events-none absolute inset-y-0 left-0 w-48 bg-linear-to-r from-black to-transparent z-10"></div>
  <div class="pointer-events-none absolute inset-y-0 right-0 w-48 bg-linear-to-l from-black to-transparent z-10"></div>

  <!-- Slider Container -->
  <div class="flex gap-16 will-change-transform" bind:this={sliderContainer}>
    {#each [...techStack, ...techStack] as tech}
      <div class="flex items-center gap-4 min-w-[150px] p-6 rounded-xl hover:bg-white/10 hover:-translate-y-1 hover:border-white/30 transition-all duration-300">
        <div class=" hover:grayscale-0 transition">
          <img src="/src/lib/assets/stack/{tech.img}" alt={tech.name} class="w-20 h-20 object-contain" />
        </div>
      </div>
    {/each}
  </div>
</section>


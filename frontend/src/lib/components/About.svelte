<script>
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/dist/ScrollTrigger";

  let header, mission, project;

  let blackBg;

  gsap.registerPlugin(ScrollTrigger);

  onMount(() => {
    // Parallax backgrounds
    [header, mission, project].forEach((section) => {
      const bg = section.querySelector(".bg");
      gsap.to(bg, {
        //yPercent: 177, // controls depth of parallax
        ease: "none",
        scrollTrigger: {
          trigger: section,
          start: "top top",
          end: "bottom -100%",
          scrub: true,
        },
      });
    });


    gsap.utils.toArray("#mission, #project").forEach((section) => {
  gsap.fromTo(section, 
    { opacity: 0 },
    {
      opacity: 1,
      duration: 1,
      ease: "power2.out",
      scrollTrigger: {
        trigger: section,
        start: "top 90%",
        end: "top top",
        scrub: true,
      },
    }
  );
});


	gsap.fromTo(blackBg, {
opacity: 0,
	}, {
		opacity:1,
		scrollTrigger: {
			trigger: header,
			start: 'bottom 150%',
			end: 'bottom 85%',
			scrub: true
		}
	})

    gsap.utils.toArray(".header-text, #mission h3, #mission p, #project h3, #project p, #project img").forEach((el) => {
      gsap.from(el, {
        opacity: 0,
        y: 40,
        duration: 1.2,
        ease: "power3.out",
        scrollTrigger: {
          trigger: el,
          start: "top 80%",
          toggleActions: "play none none reverse",
        },
      });
    });
  });
</script>

<section id="about" class="relative overflow-hidden text-white">
  <div class="relative z-20 flex flex-col">
    <!-- Header -->
    <header
      bind:this={header}
      class="relative min-h-screen flex flex-col justify-center items-center text-center px-6 md:px-16"
    >
      <div
        class="bg absolute inset-0 bg-[url('/images/bg/aboutmission.jpg')] bg-cover bg-center"
      ></div>
      <div class="absolute inset-0 bg-black/60"></div>

      <h2 class="header-text text-4xl md:text-5xl font-bold mb-4 z-10">
        Memberdayakan UMKM Lokal bersama <span class="text-base">IKU</span>
      </h2>
      <p class="header-text text-lg md:text-xl text-gray-200 max-w-2xl z-10">
        Temukan dan dukung UMKM di sekitarmu. IKU menghubungkan kamu dengan usaha kecil
        di sekitar, membantu komunitas lokal tumbuh melalui visibilitas dan kehadiran digital.
      </p>
	  <div bind:this={blackBg} class="absolute bottom-0 left-0 right-0 h-32 bg-gradient-to-b from-transparent to-white"></div>

    </header>

    <!-- Mission -->
    <section
      id="mission"
      bind:this={mission}
      class="relative min-h-screen flex flex-col justify-center items-center px-6 md:px-16 text-gray-800"
    >
      <div
        class="bg absolute inset-0  bg-center"
      ></div>
      <div class="absolute inset-0 bg-white/60 backdrop-blur-sm"></div>

      <div class="max-w-4xl text-center relative z-10 ">
        <h3 class="text-3xl font-bold mb-3">Misi Kami</h3>
        <p class="text-2xl font-semibold leading-relaxed text-gray-600">
          Kami percaya bahwa UMKM adalah tulang punggung perekonomian Indonesia.
          Melalui IKU, kami ingin menghadirkan solusi digital yang mudah digunakan
          untuk membantu UMKM mendapatkan lebih banyak pelanggan, memperluas jangkauan,
          dan meningkatkan daya saing di era digital.
        </p>
      </div>
    </section>

    <!-- Project -->
    <section
      id="project"
      bind:this={project}
      class="relative min-h-screen grid md:grid-cols-2 gap-8 items-center px-6 md:px-16"
    >
      <div class="absolute inset-0 bg-black/90"></div>

      <div class="space-y-1 text-left relative z-10">
        <h3 class="text-4xl font-semibold">Apa yang Kami <span class="text-base">Lakukan</span></h3>
        <p class="text-lg text-gray-200 leading-relaxed">
          IKU membantu pengguna menemukan UMKM terdekat melalui peta interaktif.
          Setiap pelaku usaha dapat mendaftarkan usahanya untuk tampil di peta,
          menambahkan foto, deskripsi, dan informasi kontak.
          Dengan cara ini, kami tidak hanya meningkatkan eksposur UMKM,
          tetapi juga memudahkan masyarakat untuk mengenal, mengunjungi,
          dan mendukung produk lokal di sekitar mereka.
        </p>
      </div>

      <div class="flex justify-center relative z-10">
        <img
          src="/images/bg/mapcon.jpg"
          alt="Ilustrasi proyek"
          class="rounded-2xl shadow-lg max-h-96 object-cover"
        />
      </div>
	  <div class="absolute top-0 left-0 right-0 h-32 bg-gradient-to-t from-transparent to-white"></div>

    </section>
  </div>
</section>


<style>
  
  .bg {
    transform: translateZ(0);
    will-change: transform;
  }
</style>

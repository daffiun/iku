<script>
   import { onMount } from "svelte";
   import { gsap } from "gsap";
 
   const faqs = [
     {
       question: "Bagaimana cara mendaftar sebagai pelaku UMKM di IKU?",
       answer:
         "Kamu dapat menghubungi kami melalui whatsapp yang ada dibawah ini 082231473773"
     },
     {
       question: "Apakah pendaftaran UMKM di IKU berbayar?",
       answer:
         "Tidak, saat ini pendaftaran dan penampilan usaha di platform IKU sepenuhnya gratis. Kami ingin mendukung pertumbuhan UMKM lokal tanpa hambatan biaya."
     },
     {
       question: "Bagaimana cara pengguna menemukan UMKM di sekitar mereka?",
       answer:
         "Pengguna dapat menggunakan fitur “Jelajahi Peta”, yang menampilkan berbagai UMKM di wilayah mereka. Kamu bisa mencari berdasarkan kategori, lokasi, atau nama usaha."
     },
     {
       question: "Bagaimana cara pengguna menemukan UMKM di sekitar mereka?",
       answer:
         "Pengguna dapat menggunakan fitur “Jelajahi Peta”, yang menampilkan berbagai UMKM di wilayah mereka. Kamu bisa mencari berdasarkan kategori, lokasi, atau nama usaha."
     },
     {
       question: "Bagaimana cara pengguna menemukan UMKM di sekitar mereka?",
       answer:
         "Pengguna dapat menggunakan fitur “Jelajahi Peta”, yang menampilkan berbagai UMKM di wilayah mereka. Kamu bisa mencari berdasarkan kategori, lokasi, atau nama usaha."
     }
   ];
 
   onMount(() => {
     const items = document.querySelectorAll(".faq-item");
 
     items.forEach((item) => {
       const header = item.querySelector(".faq-header");
       const icon = item.querySelector(".faq-icon");
       const content = item.querySelector(".faq-content");
 
       gsap.set(content, {
         scaleY: 0,
         opacity: 0,
         transformOrigin: "top",
         display: "none"
       });
 
       let isOpen = false;
 
       header.addEventListener("click", () => {
         const tl = gsap.timeline({ defaults: { ease: "power2.out" } });
 
         if (!isOpen) {
           tl.to(icon, {
             scale: 0.6,
             duration: 0.15,
             ease: "power1.in"
           })
             .add(() => {
               icon.textContent = "–";
               icon.style.color = "#ff6600";
             })
             .to(icon, {
               scale: 1,
               duration: 0.25,
               ease: "back.out(2)"
             })
             .set(content, { display: "block" })
             .to(
               content,
               {
                 scaleY: 1,
                 opacity: 1,
                 duration: 0.45
               },
               "<0.05"
             );
         } else {
           tl.to(icon, {
             scale: 0.6,
             duration: 0.15,
             ease: "power1.in"
           })
             .add(() => {
               icon.textContent = "+";
               icon.style.color = "#ffffff";
             })
             .to(icon, {
               scale: 1,
               duration: 0.25,
               ease: "back.out(2)"
             })
             .to(
               content,
               {
                 scaleY: 0,
                 opacity: 0,
                 duration: 0.35,
                 onComplete: () => gsap.set(content, { display: "none" })
               },
               "<"
             );
         }
 
         isOpen = !isOpen;
       });
     });
   });
 </script>
 
 <section class="min-h-screen bg-black text-white flex flex-col items-center py-20">
   <h1 class="text-4xl font-semibold mb-12 tracking-wide">FAQ</h1>
 
   <div class="w-full max-w-4xl px-4">
     {#each faqs as faq}
       <div class="faq-item border-t border-gray-700 py-6">
         <div
           class="faq-header flex items-center gap-4 cursor-pointer transition-colors"
         >
           <span class="faq-icon text-3xl font-light text-white transition-colors">
             +
           </span>
           <h3 class="text-xl font-semibold">{faq.question}</h3>
         </div>
         <div class="faq-content overflow-hidden mt-4">
           <p class="text-gray-300 leading-relaxed">{faq.answer}</p>
         </div>
       </div>
     {/each}
   </div>
 </section>
 
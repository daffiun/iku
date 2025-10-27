<section class="sec">
	<div class="bg"></div>
	<h1>Kumiko</h1>
</section>

<section class="sec">
	<div class="bg"></div>
	<h1>:3</h1>
</section>

<section class="sec">
	<div class="bg"></div>
	<h1>My</h1>
</section>

<section>
	<div class="bg"></div>
	<h1>sybau</h1>
</section>

<style>

body {
	margin: 0;
}

h1 {
	font-family: "Sawarabi Mincho", serif;
  	font-style: normal;
  	font-weight: 600;
  	word-spacing: 0.15em;
  	letter-spacing: 0.15em;
  	color: white;
  	text-shadow: 1px 1px 3px black;
}

section {
  position: relative;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  
  div {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    z-index: -1;
    background-size: cover;
    background-position: center;
    background-attachment: fixed;
    background-repeat: no-repeat;
  }
}



</style>

<script>
	import { onMount } from "svelte";
	import { gsap } from "gsap";
	
	const paths = ["ku1.jpg", "ku2.jpg", "ku3.jpg", "ku4.gif"];

	onMount(()=>{
		
	gsap.utils.toArray('section').forEach((section, i) => {
		section.bg = section.querySelector('.bg');

		section.bg.style.backgroundImage = `url(${paths[i]})`;

		if (i){
			section.bg.style.backgroundPosition = `50% ${-innerHeight / 2}px`;
			gsap.to(section.bg, {
				backgroudPosition: `50% ${innerHeight  / 2}px`,
				ease: true,
				scrollTrigger: {
					
				} 
			})
		}

		else {
			section.bg.style.backgroundPosition = `50% 0`;
			gsap.to(section.bg, {
				backgroundPosition: `50% ${-innerHeight / 2}px`,
				ease: true,
				scrollTrigger: {
					start: 'top top',
					// bottom default bottom top
				}
			})
		}

	})
	})
</script>

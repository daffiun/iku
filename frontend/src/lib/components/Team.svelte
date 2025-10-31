<script lang="ts">
  import TeamSection from '$lib/components/TeamSection.svelte';

  const members = [
    {
      id: 1,
      name: 'Daffi Achmad Wijayanto',
      role: 'Product Owner',
      photo: '/images/bg/cp3.jpg',
      bio: 'I prefer mouth to mouth conversation',
      socials: {
        github: 'https://github.com/daffiun'
      }
    },
    {
      id: 2,
      name: 'Irvan Kurniawan',
      role: 'Scrum Master',
      photo: '/images/bg/cp1.jpg',
      bio: 'Hidup jokow',
      socials: {
        github: 'https://github.com/IrvanKurniawan624'
      }
    },
    {
      id: 3,
      name: 'Hilal Rahmatullah',
      role: 'Backend Developer',
      photo: '/images/bg/cp2.jpg',
      bio: 'tch',
      socials: {
        github: 'https://github.com/redenhilal'
      }
    }
  ];
</script>

<TeamSection {members} />


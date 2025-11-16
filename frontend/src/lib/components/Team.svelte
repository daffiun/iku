<script lang="ts">
  import TeamSection from '$lib/components/TeamSection.svelte';

  const members = [
    {
      id: 1,
      name: 'Daffi Achmad Wijayanto',
      role: 'Raja',
      photo: '/images/bg/cp3.jpg',
      bio: 'I prefer face to face conversation',
      socials: {
        github: 'https://github.com/daffiun'
      }
    },
    {
      id: 2,
      name: 'Irvan Kurniawan',
      role: 'Frontend Developer',
      photo: '/images/bg/cp1.jpg',
      bio: 'Jarwis tolong itunya diapakan gitu biar gk apakali',
      socials: {
        github: 'https://github.com/IrvanKurniawan624'
      }
    },
    {
      id: 3,
      name: 'Hilal Rahmatullah',
      role: 'Frontend Developer',
      photo: '/images/bg/cp2.jpg',
      bio: 'tch',
      socials: {
        github: 'https://github.com/redenhilal'
      }
    }
  ];
</script>

<TeamSection {members} />


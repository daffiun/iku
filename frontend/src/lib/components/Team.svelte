<script lang="ts">
  import TeamSection from '$lib/components/TeamSection.svelte';

  const members = [
    {
      id: 1,
      name: 'Daffi Achmad Wijayanto',
      role: 'Product Owner',
      photo: '/images/alya.jpg',
      bio: 'I prefer mouth to mouth conversation',
      socials: {
        twitter: 'https://twitter.com/alyap',
        github: 'https://github.com/alyap'
      }
    },
    {
      id: 2,
      name: 'Irvan Kurniawan',
      role: 'Scrum Master',
      photo: '',
      bio: 'Hidup jokow',
      socials: {
        github: 'https://github.com/rafi'
      }
    },
    {
      id: 3,
      name: 'Hilal Rahmatullah',
      role: 'Backend Developer',
      photo: '',
      bio: 'tch',
      socials: {
        github: 'https://github.com/redenhilal'
      }
    }
  ];
</script>

<TeamSection {members} />


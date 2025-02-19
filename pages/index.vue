<template>
  <div class="container">
    <!-- Title -->
    <div class="mt-5 flex flex-column justify-center align-center">
      <CenteredHero
        icon="../icons/cssc-logo-without-title.svg"
        title="UTM CSSC"
        desc="Your UTM Computer Science Community Website"
        :button1="button1"
        :button2="button2"
      />
    </div>
    <a id="cssc-moto" />
    <!-- What is CSSC -->
    <div class="my-5">
      <div class="cssc-heading">
        What is CSSC?
      </div>
      <p>
        The Computer Science Student Community (CSSC) is an open space for
        Computer Science students to come in and ask questions about their
        university and post-university career. We act as liaisons to the
        Computer Science faculty to ensure that all students can seek the
        appropriate support and guidance. CSSC seeks to support students through
        community initiatives built upon inclusivity and empowerment.
      </p>
    </div>
    <CenteredThreeColumnGrid :children="features" :rounded="false" />
    <!-- Tech Team -->
    <div class="my-5">
      <div class="cssc-heading">
        Meet the Team
      </div>
      <div class="teams">
        <Teammate
          v-for="mate in currentTeam"
          :key="mate.name"
          :imgSrc="mate.imgSrc"
          :name="mate.name"
          :position="mate.position"
          :message="mate.message"
          :discord="mate.discord"
          :email="mate.email"
          :linkedin="mate.linkedin"
        />
      </div>
    </div>
    <!-- Collabutors Section -->
    <div class="my-3">
      <div class="cssc-heading">
        Contributors
      </div>
      <Contributors :contributors="contributors" />
    </div>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      features: [
        {
          title: 'Growth',
          desc:
            "CSSC is dedicated to your academic and professional growth! We're here for you with opportunities, spaces and resources to inform your computer science journey through university!",
          icon: './icons/growth.svg',
        },
        {
          title: 'Resources',
          desc:
            'Our technology team is devoted to developing processes and tools to help you succeed! We curate from across the internet and produce original content for our community!',
          icon: './icons/resources.svg',
        },
        {
          title: 'Collaboration',
          desc:
            'We work closely with other MCS clubs and societies in order to achieve common goals in the CS community!',
          icon: './icons/collaboration.svg',
        },
      ],
      button1: {
        label: 'Resources',
        link: 'resources',
      },
      button2: {
        label: 'Winter Welcome',
        link: '/winter-welcome-survey',
        // ../../resources/mcs-townhall/cssc_mcs_townhall_notes_2020.pdf
      },
    }
  },
  async asyncData({$axios, $content, params, error}) {
    const teamDataStore = await $content('team').fetch()
    const currentTeam = teamDataStore[0].team
    const contributors = []
    await $axios
      .$get('https://api.github.com/repos/utm-cssc/website/contributors')
      .then(res => {
        for (const c of res) {
          const contributor = {
            login: c.login,
            imgSrc: c.avatar_url,
            url: c.html_url,
          }
          contributors.push(contributor)
        }
      })
      .catch(e => {
        console.log(e.message)
        console.log({statusCode: 404, message: 'Something went wrong'})
      })
    return {contributors, currentTeam}
  },
}
</script>

<style scoped>
.teams {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.hero-full-height {
  height: 100vh;
}

@media screen and (max-width: 768px) {
  .hero-full-height {
    min-height: 100vh;
  }

  .teams {
    justify-content: center;
  }
}

@media (min-width: 1904px) {
  .teams {
    justify-content: initial;
  }
}
</style>

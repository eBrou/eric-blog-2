<template>
  <Layout page="info">
    <g-image
      class="info__profile-pic"
      src="~/assets/content/images/eric_bean_portrait_small.jpg"
      alt="Eric Broucek profile picture"
      width="800"
    ></g-image>
    <section
      class="info__blurb"
      :style="
        `background-color: ${data.background_color}; color: ${data.text_color}`
      "
    >
      <div v-html="data.description"></div>
      <ul>
        <li>
          <p>
            <a
              :href="`https://twitter.com/${data.contact.twitter_handle}`"
            >Twitter: @{{ data.contact.twitter_handle }}</a>
          </p>
        </li>
        <li>
          <p>
            <a
              :href="`https://github.com/${data.contact.github_handle}`"
            >Github: {{ data.contact.github_handle }}</a>
          </p>
        </li>
      </ul>
      <div class="info__fine-print" v-html="data.fine_print"></div>
    </section>
  </Layout>
</template>

<script>
export default {
  metaInfo() {
    return {
      title: "Info"
    };
  },
  computed: {
    data: function() {
      return this.$page.metaData.infoData;
    }
  }
};
</script>

<page-query>
    query getInfoPageData {
        metaData {
            infoData {
                description
                cta 
                contact {
                    email
                    twitter_handle
                    github_handle
                }
                background_color
                text_color
                fine_print
            }
        }
    }
</page-query>

<style lang="scss">
.info__blurb {
  max-width: 800px;
  padding: 1.5rem 1.25rem;
  display: flex;
  flex-direction: column;
  padding: 1.5rem 1.25rem;
  p {
    font-size: 1.4rem;
  }
}

.info__profile-pic {
  width: 250px;
  border-radius: 50%;
  margin-top: 1.5rem;
  margin-left: 2rem;
  margin: 1.5rem 2rem 0;
}

.info__fine-print {
  p {
    font-size: 0.8rem !important;
  }
}

@media (min-width: 768px) {
  .info__blurb {
    padding: 2rem;
  }
}

@media (min-width: 1440px) {
  .info__blurb {
    padding: 3rem;
  }
}
</style>

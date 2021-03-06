<template>

  <component :is="type" class="library">

    <main role="main">

      <template v-for="(call, index) in callsToAction" v-if="index === 0">
        <call-to-action :action="call.acf.action"
                        :copy="call.acf.copy"
                        :image="call.acf.image"
                        :heading="call.acf.heading"
                        :link="call.acf.link"></call-to-action>
      </template>

      <section class="background--blue-alternate library__section pb-3 pl-md-2 pb-md-4 pt-md-4 pr-md-2">

        <div class="m-md-auto d-md-flex flex-md-wrap flex-xl-nowrap justify-content-md-between col-md-10 pl-md-0 pr-md-0 pt-4 pb-4">

          <div class="card card--background-blue-dark mb-3 mb-xl-0 col-md-12 col-xl-4 mr-xl-3 p-4">

            <event-card class="card--background-blue-alternate"
                        :event="randomEvent"
                        heading-class="h4 text--white text--bold mt-4"
                        :key="randomEvent.id"
                        :truncate-excerpt="true"
                        v-if="randomEvent "/>

            <card content-container-class="p-3"
                  content-type="service"
                  :copy="randomServiceItem.description"
                  :heading="randomServiceItem.name"
                  v-if="randomServiceItem">

              <template slot="action">

                <router-link class="button button--orange" :to="`/services/${randomServiceItem.slug}`">
                  {{ randomServiceItem.acf.button_text ? randomServiceItem.acf.button_text : 'Explore'}}
                </router-link>

              </template>

            </card>

          </div>

          <collection-item class="card--background-blue-dark col-md-6 col-xl-8 mb-xl-0"
                           :item="randomCollectionItem"
                           heading-level="h3"
                           subheading-class="mt-1 text--white"
                           subheading-level="h4"
                           variant="feature"
                           v-if="randomCollectionItem" />

        </div>

      </section>

      <section class="background--white library__section p-3">

        <div class="col-md-10 d-md-flex m-auto pb-4 pl-0 pr-0 pt-4">

          <div class="col-md-8 p-0">

            <Showcase collection-link="/collection"
                      :collection-items="collection"
                      heading="New this week" />

            <card class="card--background-white text--dark"
                  content-type="blog"
                  :explainer="post.author.nice_name"
                  :sub-explainer="post.date | moment('dddd, MMMM Do')"
                  :heading="post.title"
                  v-if="post">

              <div slot="copy">
                <div v-html="post.excerpt"></div>
              </div>

              <template slot="action">
                <router-link class="button button--aqua" :to="`/posts/${post.slug}`">
                  Info
                </router-link>
              </template>

            </card>

          </div>

          <div class="col-4 p-0">
            <call-to-action action="Call to Action"
                            copy="Data tells a powerful story --
                      about your content, who reads it, and what's possible"
                            image="https://source.unsplash.com/random"
                            heading="Watch the eclipse with us"></call-to-action>
          </div>


        </div>

      </section>

    </main>

  </component>

</template>

<script>
import CallToAction from "../patterns/CallToAction.vue";
import Card from "../patterns/Card.vue";
import CollectionItem from '../patterns/CollectionItem.vue';
import EventCard from '../patterns/EventCard.vue';
import Showcase from '../patterns/Showcase.vue';

/**
 * Shows how to layout and structure a home page.
 */
export default {
  name: "Index",

  status: "prototype",

  release: "1.0.0",

  components: {
    CallToAction,
    Card,
    CollectionItem,
    EventCard,
    Showcase,
  },

  computed: {
    callsToAction() {
      return this.$store.getters.getContentByService(
        'callsToAction',
        'any',
        this.location,
      );
    },

    collection() {
      return this.$store.state.collection;
    },

    randomCollectionItem() {
      const collectionItemsWithAbstracts = this.collection
        .filter(item => item.acf.abstract !== '' && item.acf.abstract.length < 400);
      return collectionItemsWithAbstracts[
        Math.floor(Math.random() * collectionItemsWithAbstracts.length)
      ];
    },

    randomEvent() {
      return this.events[
        Math.floor(Math.random() * this.events.length)
      ];
    },

    randomServiceItem() {
      const servicesWithDescription = this.services
        .filter(service => service.description !== '');
      return servicesWithDescription[
        Math.floor(Math.random() * servicesWithDescription.length)
      ];
    },

    events() {
      return this.$store.state.events;
    },

    post() {
      return this.$store.state.posts[0];
    },

    services() {
      return this.$store.state.services;
    },
  },

  props: {
    location: {
      type: String
    },
    /**
     * The html element name used for the component.
     */
    type: {
      type: String,
      default: 'div',
    },
  },
};
</script>


<docs>
  ```jsx
  <index />
  ```
</docs>

<template lang="pug">
q-page.full-height
  section.content
    h1 Documentation

    .row.q-col-gutter-md
      .col-12.col-sm-6
        q-card
          q-card-section
            div.text-h2 Midiblocks
          q-card-section
            q-video(:ratio='16/9' src='https://player.vimeo.com/video/476536466?dnt=1')
          q-card-section
            p Learn what Midiblocks is about, how to create different kinds of apps, and more!
          q-card-actions
            q-btn.full-width(disabled) Coming soon

      .col-12.col-sm-6
        q-card
          q-card-section
            div.text-h2 Handsfree.js
          q-card-section
            q-video(:ratio='16/9' src='https://player.vimeo.com/video/476537051?dnt=1')
          q-card-section
            p Learn how to add Face Pointers and gesture recognition to your own projects (outside of Midiblocks) via CDN and NPM
          q-card-actions
            q-btn.bg-secondary.full-width(:to='{name: "DocsHandsfreeLanding"}') Explore the docs
</template>

<script>
import {set} from 'lodash'

export default {
  name: 'DocsLanding',

  mounted () {
    set(window, 'app.$page', this)
  }
}
</script>
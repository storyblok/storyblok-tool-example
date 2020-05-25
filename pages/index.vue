<template>
  <div>
    <h1>Hello world!</h1>
    <p v-if="!loadingContext">
      You are currently editing "{{ story.name }}"
    </p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      story: {},
      loadingContext: true
    }
  },

  mounted() {
    if (window.top === window.self) {
      window.location.assign('https://app.storyblok.com/oauth/tool_redirect')
    }

    window.addEventListener('message', this.processMessage, false)

    // Use getContext to get the current story
    window.parent.postMessage({action: 'tool-changed', tool: 'storyblok@first-tool', event: 'getContext'}, 'https://app.storyblok.com')
    
    // Use heightChange to change the height of the tool
    // window.parent.postMessage({action: 'tool-changed', tool: 'storyblok@first-tool', event: 'heightChange', height: 500}, 'https://app.storyblok.com')

    // Example to get the user info
    axios
      .get('/auth/user', {params: {space_id: this.$route.query.space_id}})
      .then((response) => {
        console.log(response.data)
      })

    axios
      .get(`/auth/spaces/${this.$route.query.space_id}/stories`)
      .then((response) => {
        console.log(response.data)
      })
  },
  methods: {
    processMessage(event) {
      if (event.data && event.data.action == 'get-context') {
        this.loadingContext = false
        this.story = event.data.story
      }
    }
  }
}
</script>
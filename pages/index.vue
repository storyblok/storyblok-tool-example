<template>
  <div>
    <h1>Hello world!</h1>
    <p v-if="!loadingContext">
      You are currently editing "{{ story.name }}"
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      story: {},
      loadingContext: true
    }
  },

  mounted() {
    if (window.top === window.self) {
      //window.location.assign('https://app.storyblok.com/oauth/app_redirect')
    }

    window.addEventListener('message', this.processMessage, false)

    // Use getContext to get the current story
    window.parent.postMessage({action: 'tool-changed', tool: 'storyblok@first-tool', event: 'getContext'}, 'https://app.storyblok.com')
    
    // Use heightChange to change the height of the tool
    // window.parent.postMessage({action: 'tool-changed', tool: 'storyblok@first-tool', event: 'heightChange', height: 500}, 'https://app.storyblok.com')
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
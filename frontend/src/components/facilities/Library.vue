<template>
  <links-page>
    <card v-for="(content, title) in details" :title="prettify(title)" :key="title.id">
      <div v-html="content">
      </div>
    </card>
    <card v-if="eresources" title="E-Resources">
      <ul class="list-group list-gr">
        <li v-for="eres in eresources">
          <a class="list-group-item" :href="eres.url">
            {{ eres.title }}
          </a>
        </li>
      </ul>
    </card>
  </links-page>
</template>

<script>
import Card from "@/components/Card"
import LinksPage from "@/components/LinksPage"
import axios from 'axios'
import { genBackendURL, prettify } from '@/common.js'

export default {
  name: "Library",
  data () {
    return {
      details: {},
      eresources: []
    }
  },
  created () {
    let f = false
    axios.get(genBackendURL('facilities/library'))
         .then(response => {
           this.details = response.data.results[0]
           if (response.data.results.length > 1)
             console.log("Error: More than one Library found")
           if (f)
             this.$emit('hideloader', true)
           f = true
         })
         .catch(e => {
           console.log(e)
         })
    axios.get(genBackendURL('facilities/resource'))
         .then(response => {
           this.eresources = response.data.results
           if (f)
             this.$emit('hideloader', true)
           f = true
         })
         .catch(e => {
           console.log(e)
         })
  },
  methods: {
    prettify
  },
  components: {
    Card,
    LinksPage
  }
}
</script>

<template>
  <div class="flex flex-col my-10 flex-1 shadow">
    <div class="editor-menu py-2 relative w-full border border-t-0 border-l-0 border-r-0 z-10">
      <template v-for="(Item, Index) in MenuButtons">
        <button
          class="h-full w-10"
          @click="Item.command"
          v-bind:key="Index"
          v-show="md_toggle"
          v-if="Item.hasOwnProperty('icon') || Item.hasOwnProperty('text')">
          <fa v-show="Item.icon" :icon="Item.icon" />
          <span v-show="Item.text">
            <b>{{Item.text}}</b>
          </span>
        </button>
        <dropdown v-show="md_toggle" class="px-2" v-bind:key="Index" :options="Item" v-else />
      </template>
      <button @click="() => { md_toggle = !md_toggle; }" class="h-full float-right pr-4">
        <span>
          <b>Markdown</b>
        </span>
      </button>
    </div>
    <div
      class="editor p-10 border border-white flex-1 z-0"
      ref="EditorContent"
      id="Markdown"
      contenteditable
      @input="onChange"
      v-html="html_content"
    ></div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

import showdown from 'showdown'
import MenuButtons from './_menuButtons'
import Dropdown from './Dropdown.vue'

const converter = new showdown.Converter({})

converter.setOption('tables', 'true')
converter.setOption('tasklists', 'true')
converter.setOption('metadata', 'true')
converter.setFlavor('github')

export default Vue.extend({
  props: ['value'],
  components: {
    Dropdown,
  },
  methods: {
    onChange(event: Event) {
      let target = <HTMLElement>event.target
      this.$emit('input', converter.makeMarkdown(target.innerHTML))
    },
  },
  data() {
    return {
      md_toggle: true,
    }
  },
  computed: {
    MenuButtons() {
      return MenuButtons
    },
    EditorRef() {
      return this.$refs.EditorContent
    },
    no_metadata_content() {
      if (this.value) {
        return this.value.replace(/[\+{3}](.*)[\+{3}]/gms, '')
      } else {
        return ''
      }
    },
    html_content() {
      let result = ''
      if (this.md_toggle) {
        result = converter.makeHtml(this.value)
      } else {
        result = this.value
      }
      return result
    },
  },
})
</script>

<style lang="scss">
#Markdown {
  @media print {
    *,
    *:before,
    *:after {
      background: transparent !important;
      color: #000 !important;
      box-shadow: none !important;
      text-shadow: none !important;
    }

    a,
    a:visited {
      text-decoration: underline;
    }

    a[href]:after {
      content: ' (' attr(href) ')';
    }

    abbr[title]:after {
      content: ' (' attr(title) ')';
    }

    a[href^='#']:after,
    a[href^='javascript:']:after {
      content: '';
    }

    pre,
    blockquote {
      border: 1px solid #999;
      page-break-inside: avoid;
    }

    thead {
      display: table-header-group;
    }

    tr,
    img {
      page-break-inside: avoid;
    }

    img {
      max-width: 100% !important;
    }

    p,
    h2,
    h3 {
      orphans: 3;
      widows: 3;
    }

    h2,
    h3 {
      page-break-after: avoid;
    }
  }

  pre,
  code {
    font-family: Menlo, Monaco, 'Courier New', monospace;
  }

  pre {
    padding: 0.5rem;
    line-height: 1.25;
    overflow-x: scroll;
  }

  a,
  a:visited {
    color: #3498db;
  }

  a:hover,
  a:focus,
  a:active {
    color: #2980b9;
  }

  .modest-no-decoration {
    text-decoration: none;
  }

  html {
    font-size: 12px;
  }

  @media screen and (min-width: 32rem) and (max-width: 48rem) {
    html {
      font-size: 15px;
    }
  }

  @media screen and (min-width: 48rem) {
    html {
      font-size: 16px;
    }
  }

  body {
    line-height: 1.85;
  }

  p,
  .modest-p {
    font-size: 1rem;
    margin-bottom: 1.3rem;
  }

  h1,
  .modest-h1,
  h2,
  .modest-h2,
  h3,
  .modest-h3,
  h4,
  .modest-h4 {
    margin: 1.414rem 0 0.5rem;
    font-weight: inherit;
    line-height: 1.42;
  }

  h1,
  .modest-h1 {
    margin-top: 0;
    font-size: 3.998rem;
  }

  h2,
  .modest-h2 {
    font-size: 2.827rem;
  }

  h3,
  .modest-h3 {
    font-size: 1.999rem;
  }

  h4,
  .modest-h4 {
    font-size: 1.414rem;
  }

  h5,
  .modest-h5 {
    font-size: 1.121rem;
  }

  h6,
  .modest-h6 {
    font-size: 0.88rem;
  }

  small,
  .modest-small {
    font-size: 0.707em;
  }

  /* https://github.com/mrmrs/fluidity */

  img,
  canvas,
  iframe,
  video,
  svg,
  select,
  textarea {
    max-width: 100%;
  }

  @import url('http://fonts.googleapis.com/css?family=Open+Sans+Condensed:300,300italic,700');

  @import url('http://fonts.googleapis.com/css?family=Arimo:700,700italic');

  html {
    font-size: 18px;
    max-width: 100%;
  }

  body {
    color: #444;
    font-family: 'Open Sans Condensed', sans-serif;
    font-weight: 300;
    margin: 0 auto;
    max-width: 48rem;
    line-height: 1.45;
    padding: 0.25rem;
  }

  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    font-family: Arimo, Helvetica, sans-serif;
  }

  h1,
  h2,
  h3 {
    border-bottom: 2px solid #fafafa;
    margin-bottom: 1.15rem;
    padding-bottom: 0.5rem;
    text-align: center;
  }

  blockquote {
    border-left: 8px solid #fafafa;
    padding: 1rem;
  }

  pre,
  code {
    background-color: #fafafa;
  }

  ul {
    list-style: initial;
  }

  ol {
    list-style: decimal;
  }
}
</style>


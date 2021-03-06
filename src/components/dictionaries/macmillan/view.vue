<template>
<section>
  <div class="dict-macmillan" v-if="result" @click="handleClick">
    <div v-for="res in result">
      <div class="macmillan-title" v-if="res.title">{{ res.title }}</div>
      <div class="macmillan-head">
        <star-rates :rate="res.star || 0" :width="15" :gutter="4"></star-rates>
        <span class="macmillan-head-info">{{ res.phsym }} <speaker v-if="res.audio" :src="res.audio"></speaker> {{ res.pos.toUpperCase() }} {{ res.sc }}</span>
      </div>
      <ol class="macmillan-defs" v-if="res.senses">
        <li class="macmillan-def" v-for="def in res.senses" v-html="def"></li>
      </ol>
    </div>
  </div>
</section>
</template>

<script>
import Speaker from 'src/components/Speaker'
import StarRates from 'src/components/StarRates'

export default {
  name: 'Macmillan',
  props: ['result'],
  methods: {
    handleClick ({target}) {
      let mp3 = target.dataset.srcMp3
      if (mp3) {
        chrome.runtime.sendMessage({msg: 'AUDIO_PLAY', src: mp3})
      }
    }
  },
  components: {
    StarRates,
    Speaker
  }
}
</script>

<style lang="scss">
.dict-macmillan {
  padding: 10px;
}

.macmillan-head {
  display: flex;
  margin-bottom: 5px;
}

.macmillan-head-info {
  margin-left: 10px;
  color: #aaa;
}

.macmillan-title {
  font-size: 1.3em;
  font-weight: bold;
}

.macmillan-defs {
  margin: 0;
  padding-left: 15px;
}

.macmillan-def {
  margin-bottom: 15px;

  p {
    margin: 0;
  }

  a:link,
  a:visited {
    color: #333;
    text-decoration: none;
  }

  a:hover,
  a:active {
    color: #16a085;
    border-bottom: thin dotted #16a085;
  }

  .SENSE-VARIANT,
  h3.SENSE-ENTRY,
  h2.MULTIWORD,
  h2.PHRASE-VARIANT {
    display: inline;
  }

  .SENSE-NUM,
  .foldimage {
    display: none;
  }

  .EXAMPLES {
    margin-bottom: 5px;
    padding-left: 10px;
    color: #666;
    border-left: #ddd solid 2px;
    font-style: italic;

    strong {
      font-style: normal;
    }

    a:link,
    a:visited {
      color: #666;
      text-decoration: none;
    }

    a:hover,
    a:active {
      color: #16a085;
      border-bottom: thin dotted #16a085;
    }
  }

  .EXAMPLES + .EXAMPLES {
    margin-top: -5px;
    padding-top: 3px;
  }

  .DEFINITION + .EXAMPLES {
    margin-top: 5px;
  }

  .THES {
    margin-bottom: 5px;
  }

  .icon_thesaurus_small_bullet {
    font-weight: bold;
    color: #f9690e;
  }

  .thessnippet {
    margin-left: 10px;
  }

  .SYNTAX-CODING {
    margin-right: 0.5em;
  }

  .h2 {
    margin-right: 3px;
    font-weight: bold;
  }

  .centred {
    &::before {
      content: '>';
      color: #ccc8c8;
    }
  }

  .moreButton {
    &:link,
    &:visited {
      color: #ccc8c8;
    }
  }

  .ONEBOX-HEAD {
    font-weight: bold;
    color: #f9690e;
  }

  .sideboxbody {
    margin-left: 10px;
  }

  .SUB-SENSES {
    padding-left: 16px;
  }

  .sound,
  .audio_play_button {
    width: 16px;
    vertical-align: text-bottom;
    cursor: pointer;
  }
}
</style>

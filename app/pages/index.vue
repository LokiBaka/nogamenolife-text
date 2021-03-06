<script>
  import axios from 'axios'

  const crappyClone = (payload) => JSON.parse(JSON.stringify(payload))
  const postUrl = 'https://blog.decentm.com/no-game-no-life-text-effect-css/'

  export default {
    data () {
      return {
        'showPalette':     false,
        'font':            true,
        'showing':         true,
        'countLimit':      6,
        'customText':      '',
        'postStatus':      null,
        'postPublished':   true,
        'animationPlayed': false,
      }
    },
    async mounted () {
      let res = null

      try {
        res = await axios.head(postUrl)
      } catch (error) {
        res = null

        if (error.response) {
          res = error.response
        }
      }

      this.postPublished = res.status !== 404
      this.postStatus = res
    },
    'methods': {
      toggle (item) {
        this[item] = !this[item]
      },
      validate (event) {
        const min = 1
        const max = 6
        const value = parseInt(event.target.value, 10)

        this.countLimit = Math.min(Math.max(value, min), max)

        return true
      },
      validateCustomText () {
        const lines = this.customText.split('\n')

        if (lines.length > 8) {
          this.customText = this.customText.split('\n').slice(0, 8).join('\n')
        }
      },
    },
    'computed': {
      postUrl () {
        return postUrl
      },
      lines () {
        return [
          'This world is just a crappy game without rules.',
          'As long as there\'s a way to win, Blank won\'t lose.',
          'Chess is not different than tic-tac-toe.',
          'What kind of idiot gives his enemy time to act?',
          'The only universal justice in this world is cuteness!',
          'Don\'t you dare look down on humans, Chlammy.',
        ]
      },
      line () {
        const chosenIndex = Math.floor(Math.random() * this.lines.length)

        return this.lines[chosenIndex]
      },
      randomLines () {
        return (limit) => {
          const lines = crappyClone(this.lines)
          const selectedLines = []

          for (let i = 0; i < limit; i = i + 1) {
            const chosenIndex = Math.floor(Math.random() * lines.length)

            selectedLines.push(lines[chosenIndex])
            lines.splice(chosenIndex, 1)
          }

          return selectedLines
        }
      },
    },
  }
</script>

<style lang="scss" scoped>
  @import '../scss/mixins';
  @import '../scss/variables';

  .panel {
    display: flex;
    flex-direction: row;
    justify-content: space-around;

    > * {
      position: relative;
      width: 100%;
    }
  }

  .showcase-wrapper {
    background-image: url('/nogamenolife-text/dakgisi-jibril.png');
    background-size: cover;
    background-position: top center;
    background-repeat: no-repeat;

    > * {
      width: 100%;
    }

    &:not(.font-ngnl) * {
      font-size: 1.4rem;
    }
  }

  .ngnl-text-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;

    > * {
      flex: 1;
    }

    .ngnl-text-content {
      display: flex;
      white-space: nowrap;
      color: map-get($colours, 'white');
      text-shadow:
        0 0 8px map-get($colours, 'pink'),
        0 0 12px map-get($colours, 'pink');

      > * {
        line-height: 40px;
        width: 0;
        text-align: center;
        clip-path: polygon(0 0, 100% 0, 100% 100%, 0% 100%);
      }

      &::before,
      &::after {
        opacity: 1;
        visibility: hidden;
        background-image: url('/nogamenolife-text/corner.png');
        background-size: contain;
        background-repeat: no-repeat;
        background-position: top left;
        display: block;
        min-height: 2rem;
        min-width: 2rem;
        content: '';
      }

      &::before {
        transform: translateY(-.5rem);
        align-self: flex-start;
      }

      &::after {
        transform: rotate(180deg) translateY(-.5rem);
        align-self: flex-end;
      }

      &.showing {
        &::before,
        &::after {
          @include ease-in(1s);

          visibility: visible;
          opacity: 0;
        }

        > * {
          @include ease-out(1s);

          width: 45rem;
        }
      }
    }
  }

  .credits {
    display: flex;
    flex-direction: column;
    position: absolute;
    background-color: rgba(0, 0, 0, .3);
    height: 4rem;
    width: 10rem;
    bottom: 5rem;
    right: 0;
    align-items: flex-end;
    justify-content: flex-end;
    padding: 1rem;
  }

  @keyframes pulse {
    from {
      opacity: .40;
    }

    to {
      opacity: .85;
    }
  }

  .blog-link {
    > p {
      padding-right: 1rem;

      &::before {
        margin-right: .5rem;
        transform: translateY(2px);
        display: inline-block;
        width: 1rem;
        height: 1rem;
        content: '';
        background-color: map-get($colours, 'red');
        border-radius: 50%;
        animation-name: pulse;
        animation-duration: 2s;
        animation-timing-function: ease-in-out;
        animation-play-state: running;
        animation-iteration-count: infinite;
        animation-direction: alternate;
      }
    }

    > * {
      display: inline-block;
    }
  }

  @keyframes breathe {
    0%,
    100% {
      filter: brightness(100%) saturate(100%);
    }

    50% {
      filter: brightness(80%) saturate(90%);
    }
  }

  .hint {
    @include decentm-background;

    animation-name: breathe;
    animation-duration: 2.33s;
    animation-fill-mode: both;
    animation-timing-function: ease-in-out;
    animation-iteration-count: infinite;
  }
</style>

<template lang="pug">
  .route-root
    .schemes.row.is-wrapping.foldable.has-transition(:class="{'folded': showPalette}")
      .colour-scheme-item.bg-pink.is-hcentered.is-vcentered.is-minheight-3.is-minwidth-5
        p.text-white Pink
      .colour-scheme-item.bg-red.is-hcentered.is-vcentered.is-minheight-3.is-minwidth-5
        p.text-white Red
      .colour-scheme-item.bg-purple.is-hcentered.is-vcentered.is-minheight-3.is-minwidth-5
        p.text-white Purple
      .colour-scheme-item.bg-white.is-hcentered.is-vcentered.is-minheight-3.is-minwidth-5
        p White
      .colour-scheme-item.bg-lightblue.is-hcentered.is-vcentered.is-minheight-3.is-minwidth-5
        p Lightblue
      .colour-scheme-item.bg-yellow.is-hcentered.is-vcentered.is-minheight-3.is-minwidth-5
        p Yellow

    .panel-wrapper
      .panel.is-minheight-4
        button.bg-purple.text-white(
          @click="() => {toggle('showing'); animationPlayed = true}",
          :class="{'hint': !animationPlayed}"
        )
          div(v-show="animationPlayed")
            | Show text: {{showing ? 'ON' : 'OFF'}}
          div(v-show="!animationPlayed")
            | Click here to re-run the animation!
        button.bg-yellow(@click="toggle('font')") Use font: {{font ? 'ON' : 'OFF'}}
        button.bg-red.text-white(@click="toggle('showPalette')") Show palette: {{showPalette ? 'ON' : 'OFF'}}
        button.bg-white
          label.is-vcentered.is-hcentered
            p.is-hcentered Count
            input(type="number", v-model="countLimit", min="1", max="6", @input="validate")

    .showcase-wrapper.spread(:class="{'font-ngnl': font}")
      .column
        .row.is-maxheight-7.is-hcentered.custom-text-holder
          textarea.is-maxheight-7.is-maxwidth-50.custom-text-holder(type="text", placeholder="Write your custom text here", v-model="customText", @input="validateCustomText")
        .showcase.is-vcentered.is-hcentered
          .box.is-fullwidth.is-fullheight.is-maxwidth-50.is-maxheight-20.is-hcentered.is-vcentered.bg-white
            .content.text-pink
              .ngnl-text-wrapper
                .ngnl-text-content(v-for="(entry, index) in customText.split('\\n')", v-show="customText && customText.length", :class="[{showing}, 'has-delay-' + index]")
                  h3 {{entry}}
                .ngnl-text-content(v-show="!(customText && customText.length)", v-for="(line, index) in limitBy(lines, countLimit)", :class="[{showing}, 'has-delay-' + index]")
                  h3 {{line}}
    .credits
      a.text-white(
        href="https://thekornk.deviantart.com/art/Jibril-No-Game-No-Life-639024930",
        target="_blank",
        rel="noopener nofollow"
      ) Jibril background
      a.text-white(
        href="https://japanyoshi.deviantart.com/art/No-Game-No-Life-Logo-Font-Free-DL-463798470",
        target="_blank",
        rel="noopener nofollow"
      ) Font face


    .backlink-wrapper
      .backlink
        p Made by DecentM on the 1st of April, 2018
        a(
          v-if="postPublished",
          :href="postUrl",
          rel="noopener",
          target="_blank"
        )
          button.is-brand Read how it's made!

        .blog-link(v-else)
          p I'm still writing the blog post. ({{postStatus.status}})
          a(
            href="https://blog.decentm.com/",
            rel="noopener",
            target="_blank"
          )
            button.is-brand Visit my blog!
</template>

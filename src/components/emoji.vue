<style lang="less" rel="stylesheet/less">
  .emoji-popover {
    width: 276px;
    transform: translateX(-50%);
    margin-left: 50%;
  }
  .emoji-popover .pop-body {
    max-height: 350px;
    overflow: auto;
  }
  .emoji-wrap {
    a {
      float: left;
      width: 30px;
      height: 30px;
      padding: 1px;
    }
    img {
      max-width: 100%;
    }
  }
</style>

<template>
  <div>
    <a href="javascript:;" :title="lang.title" :class="{'ve-disabled': disabled, 've-active': display}" @click="toggle">
      <i class="icon-smile-o"></i>
    </a>
    <div class="ve-popover emoji-popover" v-show="display">
      <div class="pop-arrow"></div>
      <div class="pop-header">{{lang.title}}</div>
      <div class="pop-body">
        <div class="emoji-wrap" @click="insertItem">
          <a href="javascript:;" v-for="item in arr">
            <img class="emoji" draggable="false" 
            :alt="twemoji.convert.fromCodePoint(item)" 
            :src="parseSrc(twemoji.parse(twemoji.convert.fromCodePoint(item)))">
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

// http://unicode.org/emoji/charts/full-emoji-list.html
// var arr = [];
// [].forEach.call(document.querySelectorAll('.code'), function(node){
// 	arr.push(node.children[0].getAttribute('name'))
// });
// console.log(arr);

  import twemoji from "exports?twemoji!../js/twemoji.min.js";

  export default {
    data () {
      return {
        width: 260,
        height: 180,
        twemoji,
        arr: this.$store.state.config.emoji,
        lang: this.$store.state.lang.emoji
      }
    },
    computed: {
      disabled: function () {
        return this.$store.state.toolbarStates.emoji.disabled;
      },
      display: function () {
        return this.$store.state.toolbarStates.emoji.showPopup;
      }
    },
    methods: {
      updatePopupDisplay (current) {
        this.$store.dispatch('updatePopupDisplay', current);
      },
      toggle () {
        !this.disabled && this.updatePopupDisplay('emoji');
      },
      parseSrc (str) {
        let div = document.createElement('div');
        div.innerHTML = str;
        return div.querySelector('img').src;
      },
      insertItem (event) {
        let img = event.target.cloneNode(true);
        img.style.width = '30px';
        this.$store.dispatch('execCommand', {name: 'insertHTML', value: img.outerHTML});
      }
    }
  }
</script>
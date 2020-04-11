<template>
  <div>
    <div>
       <button @click="saveSvg">保存</button>
    </div>
    <div class="svgedit__wrapper">
      <iframe ref="iframe" src="./svg-edit.html" width="1300px" height="600px" id="svgedit" @load="initEmbed"></iframe>
    </div>
  </div>
</template>

<script>
import './package/libs/svg-edit/jquery.js'
import './package/libs/svg-edit/embedapi.js'

export default {
  name: 'svg-edit-component',
  components: {
  },
  props: {
    content: {
      type: String,
      default: ''
    },
    save: {
      type: Function,
      default: null
    }
  },
  data () {
    return {
      svgCanvas: null,
      frame: null
    }
  },
  mounted () {
    this.frame = this.$refs.iframe

  },
  methods: {
    /* 用正则表达式实现html编码（转义）*/
    html2Escape:function(sHtml) {
      return sHtml.replace(/[<>&"]/g,function(c){return {'<':'&lt;','>':'&gt;','&':'&amp;','"':'&quot;'}[c];});
    },
    /* 用正则表达式实现html解码（反转义）*/
    escape2Html:function (str) {
      let arrEntities={'lt':'<','gt':'>','nbsp':' ','amp':'&','quot':'"'};
      return str.replace(/&(lt|gt|nbsp|amp|quot);/ig,function(all,t){return arrEntities[t];});
    },
    initEmbed() {
        let doc, mainButton;
        let frame = this.frame
        this.$nextTick(() => {
          if (frame) {
            this.svgCanvas = new EmbeddedSVGEdit(frame)
            doc = frame.contentDocument || frame.contentWindow.document;
            mainButton = doc.getElementById('main_button')
            mainButton && (mainButton.style.display = 'none')
            this.loadSvg()
          }
        })
    },
    loadSvg() {
      let content = this.content
      if (content && content.length !== 0) {
        this.svgCanvas && this.svgCanvas.setSvgString(this.escape2Html(this.content));
      }
    },
    saveSvg() {
        this.svgCanvas && this.svgCanvas.getSvgString()(this.handleSvgData);
    },
    handleSvgData(data, error) {
        let save = this.save
        let result = error ? null : this.html2Escape(data)
        if (typeof save ===  'function') {
          save(result, error)
        } else {
          this.$emit('save', result, error)
        }
    }
  }
}
</script>
<style>
.iframe_wrapper {
  width: 100%;
  height: 100%;
}
* {
		margin: 0;
		padding: 0;
	}
	html, body {
      width: 100%;
      height: 100%;
      /* height: calc(100% - 50px); */
	}
    .svgedit__wrapper {
        position: absolute;
        top: 50px;
        width: 100%;
        bottom: 0;
        left: 0;
        right: 0;
    }
	#svgedit {
	  border: none;
      width: 100%;
      height: 100%;
	}
</style>

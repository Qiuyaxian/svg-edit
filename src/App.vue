<template>
  <div id="app">
    <svg-edit-component :content="data" :save="save"></svg-edit-component>
  </div>
</template>

<script>
import SvgEditComponent from './components/svg-edit'

export default {
  name: 'app',
  components: {
    SvgEditComponent
  },
  data() {
    return {
      data: `&lt;svg width=&quot;640&quot; height=&quot;480&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:svg=&quot;http://www.w3.org/2000/svg&quot;&gt;
      &lt;!-- Created with SVG-edit - http://svg-edit.googlecode.com/ --&gt;
      &lt;g&gt;
        &lt;title&gt;Layer 1&lt;/title&gt;
        &lt;line id=&quot;svg_1&quot; y2=&quot;179&quot; x2=&quot;156&quot; y1=&quot;60&quot; x1=&quot;93&quot; stroke-width=&quot;5&quot; stroke=&quot;#000000&quot; fill=&quot;none&quot;/&gt;
      &lt;/g&gt;
      &lt;/svg&gt;`
    }
  },
  methods: {
    save(data, error) {
      console.log(data, 'sd')
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
* {
		margin: 0;
		padding: 0;
	}
	html, body {
		width: 100%;
		height: 100%;
	}
	#iframe-wrapper {
		border: none;
	}
</style>

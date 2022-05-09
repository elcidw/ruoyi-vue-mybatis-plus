<template>
  <div>
    <div id="canvas" ref="canvas" class="canvas" />
    <div>
      <el-button type="primary" size="small" @click="save">保存SVG</el-button>
    </div>
    <Panel ref="panel" :element="element" :business-object="businessObject" :moddle="moddle" :modeling="modeling" />
  </div>
</template>

<script >
import 'bpmn-js/dist/assets/diagram-js.css'
import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-embedded.css';
import Modeler from 'bpmn-js/lib/Modeler';
import Panel from './Panel.vue';
import { saveAs } from 'file-saver';
import { getBusinessObject } from 'bpmn-js/lib/util/ModelUtil';


export default {
  name: 'Modeler',
  components: {
    Panel
  },
  data() {
    return {
      bpmnModeler: null,
      element: null,
      businessObject: null,
      moddle: null,
      modeling: null,
      panel: null
    }
  },
  mounted() {
    this.init()
    this.bpmnModeler.on('element.click', (event) => {
      event.originalEvent.preventDefault()
      event.originalEvent.stopPropagation()
      console.log('event: ', event.element)
      this.element = event.element
      this.businessObject = getBusinessObject(this.element)
      console.log('businiessObject: ', this.businessObject)
      console.log('businessObject', this.businessObject.$type) // 流程节点的类型
      this.$nextTick(function(){
        console.log(this.$refs.panel)
        this.$refs.panel.init()
      })
    })
  },
  methods: {
    init() {
      this.bpmnModeler = new Modeler({
        container: '#canvas',
        height: '100%'
      })
      try {
        console.log('modeler');

        const result = this.bpmnModeler.createDiagram();
        const { warnings } = result
        console.log(warnings)
      } catch (err) {
        console.log(err.message, err.warnings);
      }
    },
    save() {
      try {
        const result = this.bpmnModeler.saveSVG()
        const { svg } = result
        const str = new Blob([svg], { type: 'text/plain;charset=utf8' })
        saveAs(str, 'test.svg')
        console.log(result)
      } catch (err) {
        console.log(err)
      }
    }
  }
}

</script>

<style>
.canvas {
  height: 701px;
  background-image: linear-gradient(to right, #ccc 1px, transparent 1px),
    linear-gradient(to bottom, #ccc 1px, transparent 1px);
  background-repeat: repeat;
  /* 默认为 repeat */
  background-size: 10px 10px;
}
</style>
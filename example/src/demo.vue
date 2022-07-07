<template>
  <div>
    <hsc-menu-style-metal style="position: fixed; z-index: 2;">
      <hsc-menu-bar style="border-radius: 0 0 4pt 0">
        <hsc-menu-bar-item label="开始">
          <hsc-menu-item label="全局检视" @click="newDemoWindow('top')" />
          <hsc-menu-item label="局部检视">
	    <hsc-menu-item label="MPstat (CPU多核状态)" @click="newDemoWindow('mpstat')" />
	    <hsc-menu-item label="PIDstat (进程状态)" @click="newDemoWindow('pidstat')" />
	    <hsc-menu-item label="VMstat (内存与上下文切换)" @click="newDemoWindow('vmstat')" />
          </hsc-menu-item>
          <hsc-menu-item label="分析工具">
            <hsc-menu-item label="speedscope" @click="newDemoWindow('speedscope')" />
          </hsc-menu-item>
          <hsc-menu-item label="Bash" @click="newDemoWindow('bash')" />
	  <hsc-menu-separator/>
          <hsc-menu-item label="Clear" keybind="alt+k" @click="demoWindows=[]" />
        </hsc-menu-bar-item>
        <hsc-menu-bar-item label="窗口">
          <hsc-menu-item v-for="w of demoWindows" :key="w.id" :label="w.label" @click="w.isOpen=!w.isOpen" :checked="w.isOpen" />
        </hsc-menu-bar-item>
      </hsc-menu-bar>
    </hsc-menu-style-metal>

    <hsc-window-style-metal style="position: fixed; z-index: 1">
      <hsc-window v-for="w of demoWindows" :key="w.id" :resizable="true" :title="w.label" :closeButton="true" :isOpen.sync="w.isOpen" :width="650" :height="400">
      	<iframe :src="w.srcPath" style="width: 100%; height: 100%; box-sizing: border-box; border-style: none;" />
      </hsc-window>
    </hsc-window-style-metal>
  </div>
</template>


<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'
import * as VueMenu from '@hscmap/vue-menu'
import * as _ from 'lodash'


Vue.use(VueMenu)

export class DemoWindow {
  private static id = 0

  name = ""
  srcPath = ""
  id = DemoWindow.id++
  isOpen = true

  constructor(name: string) {
        let path = "http://localhost:8080/"
        if (name === "speedscope") { 
            path = "https://www.speedscope.app/"
        } else {
            path = path + name
        }
	this.name = name
	this.srcPath = path
  }

  get label() {
    return `${this.name}`
  }
}


@Component
export default class Demo extends Vue {
  demoWindows: DemoWindow[] = []//_.range(3).map(i => new NumberWindow())

  newDemoWindow(name: string) {
    this.demoWindows.push(new DemoWindow(name))
  }
}
</script>


<style lang="scss" scoped>
table {
  white-space: nowrap;
  border-spacing: 0.5em;
}

td,
th {
  text-align: center;
  padding: 1em;
  box-shadow: 0 0 4pt rgba(0, 0, 0, 0.25);
  background-color: #eee;
  border-radius: 4pt;
}
</style>

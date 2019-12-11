<template>
  <div id="app">
    <i-layout vertical>
      <i-layout-aside class="_text-center">
        <h2>Step 1</h2>
        Select One Image<br>
        <input type="file" id="getval" @change="imgTrigger">
        <div id="imgOri" style="width:100%;height:500px;"></div>
      </i-layout-aside>
      
      <i-layout>
          <i-layout-header class="_text-center">
            <h2>Step 2</h2>
          </i-layout-header>
          
          <i-layout-content class="_text-center">
            <h4>Filter List</h4>
            <i-button outline variant="dark" @click="switcher('grayscale')">Greyscale</i-button><br><br>
            <i-button outline variant="dark" @click="switcher('sephia')">Sephia</i-button><br><br>
          </i-layout-content>
          
          <i-layout-footer class="_text-center">
            imgconvert &copy; 2019
          </i-layout-footer>
      </i-layout>
      
      <i-layout-aside class="_text-center">
        <h2>Step 3</h2>
        Result<br>
        <i-button outline variant="success" @click="getImage">Download</i-button><br>
        <div id="resultImg" style="width:100%;height:500px;" :class="{ grayscale: isGrayscale, sephia: isSephia }"></div>
      </i-layout-aside>
    </i-layout>
  </div>
</template>

<script>
import domtoimage from 'dom-to-image'
import { saveAs } from 'file-saver'
export default {
  name: 'app',
  data() {
    return {
      isGrayscale: false,
      isSephia: false
    }
  },
  methods: {
    imgTrigger(){
      let file = document.getElementById("getval").files[0]
      let reader = new FileReader()
      reader.onloadend = function() {
        document.getElementById('imgOri').style.backgroundImage = "url(" + reader.result + ")"
        document.getElementById('imgOri').style.backgroundRepeat = 'no-repeat'
        document.getElementById('imgOri').style.backgroundSize = 'contain'
        document.getElementById('imgOri').style.backgroundPosition = 'center'
        document.getElementById('resultImg').style.backgroundImage = "url(" + reader.result + ")"
        document.getElementById('resultImg').style.backgroundRepeat = 'no-repeat'
        document.getElementById('resultImg').style.backgroundSize = 'contain'
        document.getElementById('resultImg').style.backgroundPosition = 'center'
      }
      if(file){
        reader.readAsDataURL(file)
      }
    },
    switcher(key){
      switch (key) {
        case 'grayscale':
            this.isGrayscale = true
            this.isSephia = false
          break;
        case 'sephia':
            this.isGrayscale = false
            this.isSephia = true
          break;
      }
    },
    getImage(){
      let node = document.getElementById('resultImg')
      domtoimage.toBlob(node)
        .then(function (blob) {
          saveAs(blob, 'result.png')
        }
      )
    }
  }
}
</script>

<style>
.grayscale {
  -webkit-filter: grayscale(100%); /* Safari 6.0 - 9.0 */
  filter: grayscale(100%);
}
.sephia {
  filter: sepia(60%);
}
.layout-aside {
  width: 40rem!important;
}
</style>
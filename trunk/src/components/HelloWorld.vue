<template>
  <div class="hello">
    <div class="el-textarea" style="position: relative; width: 306px; height: 206px; overflow: hidden;" contenteditable="true">
      <textarea name="textarea" id="textarea" style="width: 300px; height: 200px;" border="1"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted() {
    document.getElementById('textarea').onpaste = () => {
      this.pasteTable(event);
    }
  },
  methods: {
    createNode (str) {
      let doc = new DOMParser().parseFromString(str, 'text/html');
      let table = doc.querySelector('#clipTable');
      return table;
    },
    pasteTable(e) {
      let that = this
      if ( e.clipboardData ) {
        console.log(e.clipboardData.getData('Text'))
        var clipData = e.clipboardData.getData('Text')
        var data = clipData.split('\n')
        var dataHtml = ''
        var allData = []
        for (var i = 0; i < data.length; i++) {
          var eachData = data[i].split('\t')
          allData.push(eachData)
        }
        console.log('allData', allData)
        if (allData.length === 1) {   //如果长度为1，说明只是复制的文本
          return false
        } else if (allData.length > 1) {   //如果长度超过1，说明复制的是表格
          // let tableDom = document.createElement('table')
          // tableDom.id = 'clipTable'
          // document.getElementById('editable').appendChild(tableDom)
          var dataHtml = '<table id="clipTable" border="1" cellpadding="0" cellspacing="0">'
          allData.forEach((item, index) => {
            dataHtml += '<tr class="datarow">'
            item.forEach((el, i) => {
              dataHtml += '<td>' + (el || '&nbsp;') + '</td>'
            })
            dataHtml += '</tr>'
          })
          dataHtml += '</table>'
          console.log(that.createNode(dataHtml))
          document.getElementsByClassName('el-textarea')[0].appendChild(that.createNode(dataHtml))
          document.getElementById('clipTable').style = 'position: absolute; left: 2px; top: 2px; max-width: 300px; max-height: 200px; font-size: 12px'
          // console.log('value', document.getElementById('textarea').value)
          setTimeout(() => {
            document.getElementById('textarea').value = ''
          }, 0);
          that.$nextTick(() => {
            const tableH = document.getElementById('clipTable').offsetHeight;
            console.log('tableH', tableH)
            if (tableH > 200) {

            }
          })
        }
      } else {
        console.log('non-chrome');
      }
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

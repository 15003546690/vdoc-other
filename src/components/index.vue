<template>
  <div class="hello">
    <!-- <div class="title">{{docName}}</div> -->
    <quill-editor v-model="docContent"
          :disabled=disabled
          ref="myQuillEditor"
          :options="editorOption"
          @blur="handleEditorBlur($event)"
          @focus="handleEditorFocus($event)"
          @change="handleEditorChange($event)">
    </quill-editor>
  </div>
</template>
<script>
import toolbarOptions from './edit.js';
export default {
  created(){
    if(this.$route.query.docId!=undefined){
      this.currentData(this.$route.query.docId);
    }
    
  },
  data () {
    return {
      docName:'',
      disabled:true,
      docContent:'',
      editorOption: {
        placeholder: '',
        theme: 'snow',  // or 'bubble'
        modules: {
          toolbar: {
              container: toolbarOptions,  // 工具栏
              handlers: {
                  'image': function (value) {
                  if (value) {
                    document.querySelector('.avatar-uploader input').click()
                  } else {
                    this.quill.format('image', false);
                  }
                }
              }
          }
        }
      },
      handleInputChange () {},
      handleEditorBlur () {},
      handleEditorFocus () {},
      handleEditorChange (e) {
        /*this.textLen = e.text.length - 1;
        this.beyond = this.textLen - 350;
        if (this.textLen > 350) {
          this.info.evId = '';
        } else {
          this.info.evId = this.evList[0].evId;
        }*/
      },
    }
  },
  methods:{
    currentData(docId){
      this.$CPOST('/noAuth/browDoc',{
        docId
      },(res)=>{
        let data=res.data;
        this.docName=data.docName;
        this.docContent=data.docContent;
      })
    }
  }
}
</script>

<style>
  .ql-toolbar{
    display:none;
  }
  .ql-container.ql-snow{
    border: none !important;
  }
</style>
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
.title{
  margin-bottom: 20px;
  text-align: left;
  font-weight: bold;
}
</style>

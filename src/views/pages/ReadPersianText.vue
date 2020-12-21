<template>
 <vx-card>
   <h2>{{ $t('ReadPersianText.header')}} </h2>
   <hr>
   <div>
  <VuePerfectScrollbar class="scroll-area p-4" :settings="settings">
    <form
        @submit.prevent
        title="New Text"
        accept-text= "Send"
        @cancel="clearFields"
        @accept="sendText"
        @close="clearFields">
        <textarea v-model="text" :options="editorOption" class="textarea" />
        <button class="btn btn-primary button" type="submit" @click="sendText()">
            <span class="text-lg ml-8">Send  </span>
            <feather-icon icon="SendIcon"></feather-icon>
        </button>
   </form>
  </VuePerfectScrollbar>

   </div>
 </vx-card>
</template>
<script>
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import { quillEditor } from 'vue-quill-editor'
import VuePerfectScrollbar from 'vue-perfect-scrollbar'
import VxCard from '../../components/vx-card/VxCard.vue'
import axios from 'axios'
import Textarea from '../forms/form-elements/textarea/Textarea.vue'
export default {
  data(){
    return{
      text: '',

      editorOption: {
        modules: {
          toolbar: [
            ['bold', 'italic', 'underline', 'strike', 'link', 'blockquote', 'code-block'],
            [{ 'header': 1 }, { 'header': 2 }],
            [{ 'list': 'ordered' }, { 'list': 'bullet' }],
            [{ 'font': [] }],
          ],
        },
        placeholder: 'Enter your Text'
      },
       settings: {
        maxScrollbarLength: 60,
        wheelSpeed: 0.30,
      },

    }
  },
components:{
  VxCard,
  quillEditor,
  VuePerfectScrollbar,
Textarea
},
methods:{
  clearFields() {
      this.$nextTick(() => {
        this.text = ''
      })
    },
    sendText() {
      var Text = {
        header: 'salam',
        paragraph: this.text
      }
      console.log(this.text);

      axios.post('https://readpersiantext-default-rtdb.firebaseio.com/data.json', Text );
      axios.get('gs://readpersiantext.appspot.com/salam.mp3')
      .then(
        res =>{

          console.log(res);
        }
      )
      this.clearFields();
    },
}
}
</script>
<style>
.textarea{
  width: 80%;
  height: 400px;
  margin: 80px;
  font: inherit;
  font-size: 20px;
  border: 3px solid orange;
  border-radius: 30px 0 30px 0;
  padding: 20px ;
}
.button{
  margin-left:100px;
  padding:5px;
}
</style>

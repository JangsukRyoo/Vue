<template>
  <div>
    <input type="checkbox" v-model="show">
    <input v-model.number="page" type="number" style="width: 5em"> /{{numPages}}
    <button @click="rotate += 90">&#x27F3;</button>
    <button @click="rotate -= 90">&#x27F2;</button>
    <button @click="$refs.pdf.print()">print</button>
    <div style="width: 50%">
      <div v-if="loadedRatio > 0 && loadedRatio < 1" style="background-color: green; color: white; text-align: center" :style="{ width: loadedRatio * 100 + '%' }">{{ Math.floor(loadedRatio * 100) }}%</div>
      <pdf v-if="show" ref="pdf" style="border: 1px solid red" :src="src" :page="page" :rotate="rotate" @password="password" @progress="loadedRatio = $event" @error="error" @num-pages="numPages = $event" @link-clicked="page = $event"></pdf>
    </div>
  </div>
</template>
<script>
import pdf from 'vue-pdf'

export default {
  components: {
    pdf: pdf
  },
  data () {
    return {
      show: true,
      pdfList: [
        '',
        'https://cdn.rawgit.com/mozilla/pdf.js/c6e8ca86/test/pdfs/annotation-link-text-popup.pdf',
      ],
      src:'',
      loadedRatio: 0,
      page: 1,
      numPages: 0,
      rotate: 0,
    }
  },
  methods: {
    password: function(updatePassword) {

      updatePassword(prompt('password is "test"'));
    },
    error: function(err) {

      console.log(err);
    }
  }
}
</script>
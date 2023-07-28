<template>
  <div>
    <input type="file" @change="onFileChange" accept="image/*" />
    <div v-if="ocrText">
      <h3>Texto reconhecido:</h3>
      <p>{{ ocrText }}</p>
    </div>
  </div>
</template>

<script>
import Tesseract from 'tesseract.js';

export default {
  data() {
    return {
      ocrText: '',
    };
  },
  methods: {
    onFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        this.performOCR(file);
      }
    },
    performOCR(file) {
      Tesseract.recognize(
          file,
          'por',
          { logger: info => console.log(info) } // Optional logger for debug
      ).then(({ data: { text } }) => {
        this.ocrText = this.removeSpecialCharacters(text);
      }).catch(error => {
        console.error('Error during OCR:', error);
      });
    },
    removeSpecialCharacters(text) {
      // Substitui caracteres especiais por espaço em branco.
      const regex = /[^a-zA-Z0-9áàâãéèêíïóôõöúçñ\s]/g;
      return text.replace(regex, " ");
    },
  },
};
</script>

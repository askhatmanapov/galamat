<template>
  <div>
    <input type="file" @change="handleFileSelect" />
    <!-- <p>{{ extractedText }}</p> -->
    <div>
      <table v-if="res1">
        <caption>{{ name }}: {{ age }}г</caption>
        <tbody>
          <tr>
            <th>Компонент</th>
            <th>Результат</th>
            <th>Норма</th>
          </tr>
          <tr>
            <td>ТТГ</td>
            <td>{{res1}}</td>
            <td>0,40 - 3,77</td>
          </tr>
          <tr>
            <td>Свободный Т3</td>
            <td>{{res2}}</td>
            <td>2,00 - 4,40</td>
          </tr>
          <tr>
            <td>Свободный Т4</td>
            <td>{{res3}}</td>
            <td>1,00 - 1,70</td>
          </tr>
          <tr>
            <td>Анти ТПО</td>
            <td>{{res4}}</td>
            <td>0 - 34</td>
          </tr>
          <tr>
            <td>Анти ТГ</td>
            <td>{{res5}}</td>
            <td>0 - 115</td>
          </tr>
          <tr>
            <td>ПСА общий</td>
            <td>{{res6}}</td>
            <td>0,000 - 2,000</td>
          </tr>
          <tr>
            <td>Индекс свободного ПСА</td>
            <td>{{res7}}</td>
            <td>выше 15</td>
          </tr>
          <tr>
            <td>ПСА свободный</td>
            <td>{{res8}}</td>
            <td></td>
          </tr>
          <tr>
            <td>Тиреоглобулин</td>
            <td>{{res9}}</td>
            <td>1,4 - 78</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import * as pdfjsLib from 'pdfjs-dist';
const pdfjsWorker = await import('pdfjs-dist/build/pdf.worker.entry');

export default {
  data() {
    return {
      extractedText: '',
      name: '',
      age: '',
      size: '',
      res1: '',
      res2: '',
      res3: '',
      res4: '',
      res5: '',
      res6: '',
      res7: '',
      res8: '',
      res9: '',
    }
  },
  methods: {
    handleFileSelect(event) {
      const file = event.target.files[0]
      const reader = new FileReader();
      reader.onload = () => {
          const pdfData = new Uint8Array(reader.result);
          pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker;
          pdfjsLib.getDocument(pdfData).promise.then((pdf) => {
            let pageTextPromises = []
            for (let i = 1; i <= pdf.numPages; i++) {
              pageTextPromises.push(pdf.getPage(i).then((page) => {
                return page.getTextContent()
              }))
            }
            Promise.all(pageTextPromises).then((pages) => {
              let text = ''
              pages.forEach((page) => {
                page.items.forEach((item) => {
                  text += item.str + ' '
                })
              })
              this.extractedText = text
              this.size = this.extractedText.length
              let id = 0
              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='ё' && this.extractedText[i+1] == 'т' && this.extractedText[i+2]=='а'){
                  id = i + 5;
                }
              }
              while (this.extractedText[id+2] != 'Г' && this.extractedText[id+3] != 'N' && this.extractedText[id+4] != 'o') {
                this.name = this.name + this.extractedText[id]
                id++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='Ж' && this.extractedText[i+1] == 'ы' && this.extractedText[i+2]=='н' && this.extractedText[i+3] == 'ы' && this.extractedText[i+4]=='с'){
                  id = i - 11;
                }
              }
              while (this.extractedText[id+1] != 'Ж' && this.extractedText[id+2] != 'ы' && this.extractedText[id+3] != 'н') {
                this.age = this.age + this.extractedText[id]
                id++;
              }

              let n = 0
              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='Т' && this.extractedText[i+1] == 'Т' && this.extractedText[i+2]=='Г'){
                  id = i + 8;
                }
              }
              while (n<12) {
                this.res1 = this.res1 + this.extractedText[id]
                id++;
                n++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]==' ' && this.extractedText[i+1] == 'Т' && this.extractedText[i+2]=='3'){
                  id = i + 8;
                  n = 0;
                }
              }
              while (n<10) {
                this.res2 = this.res2 + this.extractedText[id]
                id++;
                n++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]==' ' && this.extractedText[i+1] == 'Т' && this.extractedText[i+2]=='4'){
                  id = i + 8;
                  n = 0;
                }
              }
              while (n<10) {
                this.res3 = this.res3 + this.extractedText[id]
                id++;
                n++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='Т' && this.extractedText[i+1] == 'П' && this.extractedText[i+2]=='О'){
                  id = i + 8;
                  n = 0;
                }
              }
              while (n<8) {
                this.res4 = this.res4 + this.extractedText[id]
                id++;
                n++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]==' ' && this.extractedText[i+1] == 'Т' && this.extractedText[i+2]=='Г'){
                  id = i + 8;
                  n = 0;
                }
              }
              while (n<8) {
                this.res5 = this.res5 + this.extractedText[id]
                id++;
                n++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='и' && this.extractedText[i+1] == 'й' && this.extractedText[i+3]=='1'){
                  id = i + 7;
                  n = 0;
                }
              }
              while (n<11) {
                this.res6 = this.res6 + this.extractedText[id]
                id++;
                n++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i-18]=='И' && this.extractedText[i] == 'П' && this.extractedText[i+1]=='С' && this.extractedText[i+2]=='А'){
                  id = i + 6;
                  n = 0;
                }
              }
              while (n<7) {
                this.res7 = this.res7 + this.extractedText[id]
                id++;
                n++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i-12]=='П' && this.extractedText[i-11] == 'С' && this.extractedText[i-10]=='А' && this.extractedText[i]=='й'){
                  id = i + 6;
                  n = 0;
                }
              }
              while (n<10) {
                this.res8 = this.res8 + this.extractedText[id]
                id++;
                n++;
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='б' && this.extractedText[i+1] == 'у' && this.extractedText[i+2]=='л' && this.extractedText[i+3]=='и'){
                  id = i + 10;
                  n = 0;
                }
              }
              while (n<11) {
                this.res9 = this.res9 + this.extractedText[id]
                id++;
                n++;
              }
            })
          });
      }
      reader.readAsArrayBuffer(file);
    }
  }
}
</script>
<style>
table{
  width: 50%;
  margin-top: 20px;
  margin-left: 5%;
  margin-right: 5%;
}
td, th{
  padding: 5px;
  text-align: center;
}
</style>
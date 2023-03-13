<template>
  <div>
    <div class="upload">
      <input type="file" @change="handleFileSelect" />
    </div>
    <div class="pdf" v-if="res1">
      <table>
        <caption>{{ name }}: {{ age }}г</caption>
        <tbody>
          <tr>
            <th>Компонент</th>
            <th>Результат</th>
            <th>Норма</th>
            <th>Комментарий</th>
          </tr>
          <tr>
            <td>ТТГ</td>
            <td>{{ ttg }}</td>
            <td>0,40 - 3,77</td>
            <td></td>
          </tr>
          <tr>
            <td>Свободный Т3</td>
            <td>{{t3}}</td>
            <td>2,00 - 4,40</td>
            <td></td>
          </tr>
          <tr>
            <td>Свободный Т4</td>
            <td>{{t4}}</td>
            <td>1,00 - 1,70</td>
            <td></td>
          </tr>
          <tr>
            <td>Анти ТПО</td>
            <td>{{tpo}}</td>
            <td>0 - 34</td>
            <td></td>
          </tr>
          <tr>
            <td>Анти ТГ</td>
            <td>{{tg}}</td>
            <td>0 - 115</td>
            <td></td>
          </tr>
          <tr>
            <td>ПСА общий</td>
            <td>{{psao}}</td>
            <td>0,000 - 2,000</td>
            <td></td>
          </tr>
          <tr>
            <td>Индекс свободного ПСА</td>
            <td>{{ispsa}}</td>
            <td>выше 15</td>
            <td></td>
          </tr>
          <tr>
            <td>ПСА свободный</td>
            <td>{{psas}}</td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td>Тиреоглобулин</td>
            <td>{{tireoglubin}}</td>
            <td>1,4 - 78</td>
            <td></td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="survey">
      <div class="q1" v-if="status==1">
        <h4>1. Чувствуете ли вы слабость?</h4>
        <ul class="options">
            <input id="1-1" type="radio" name="q1" value="Эндокринолог" v-model="option1"><label for="1-1">Усталость</label><br>
            <input id="1-2" type="radio" name="q1" value="Эндокринолог" v-model="option1"><label for="1-2">Апатия</label>
        </ul>
        <button type="button" v-on:click="options.push(option1)" @click="check">Дальше</button>
    </div>

    <div class="q2" v-if="status==2">
        <h4>2. Чувствуете ли вы депрессию?</h4>
        <ul class="options">
            <input id="2-1" type="radio" name="q2" value="Эндокринолог" v-model="option2"><label for="2-1">Да</label><br>
            <input id="2-2" type="radio" name="q2" value="Невропатолог" v-model="option2"><label for="2-2">Нет</label>
        </ul>
        <button type="button" v-on:click="options.push(option2)" @click="check">Дальше</button>
    </div>

    <div class="q3" v-if="status==3">
        <h4>3. Чувствуете ли вы бессонницу?</h4>
        <ul class="options">
            <input id="3-1" type="radio" name="q3" value="Невропатолог" v-model="option3"><label for="3-1">Да</label><br>
            <input id="3-2" type="radio" name="q3" value="Эндокринолог" v-model="option3"><label for="3-2">Нет</label>
        </ul>
        <button type="button" v-on:click="options.push(option3)" @click="check">Дальше</button>
    </div>

    <div class="q4" v-if="status==4">
        <h4>4. Чувствуете ли вы зябкость?</h4>
        <ul class="options">
            <input id="4-1" type="radio" name="q4" value="Эндокринолог" v-model="option4"><label for="4-1">Утром</label><br>
            <input id="4-2" type="radio" name="q4" value="Кардиолог" v-model="option4"><label for="4-2">Вечером</label><br>
            <input id="4-3" type="radio" name="q4" value="Кардиолог" v-model="option4"><label for="4-3">Постоянно</label>
        </ul>
        <button type="button" v-on:click="options.push(option4)" @click="check">Дальше</button>
    </div>

    <div class="q5" v-if="status==5">
        <h4>5. Чувствуете ли вы вялость?</h4>
        <ul class="options">
            <input id="5-1" type="radio" name="q5" value="Эндокринолог" v-model="option5"><label for="5-1">Утром</label><br>
            <input id="5-2" type="radio" name="q5" value="Кардиолог" v-model="option5"><label for="5-2">Вечером</label><br>
            <input id="5-3" type="radio" name="q5" value="Невропатолог" v-model="option5"><label for="5-3">Постоянно</label>
        </ul>
        <button type="button" v-on:click="options.push(option5)" @click="check">Дальше</button>
    </div>

    <div class="q6" v-if="status==6">
        <h4>6. Когда вы чувствуете сонливость?</h4>
        <ul class="options">
            <input id="6-1" type="radio" name="q6" value="Эндокринолог" v-model="option6"><label for="6-1">Утром</label><br>
            <input id="6-2" type="radio" name="q6" value="Эндокринолог" v-model="option6"><label for="6-2">Вечером</label><br>
            <input id="6-3" type="radio" name="q6" value="Невропатолог" v-model="option6"><label for="6-3">Постоянно</label>
        </ul>
        <button type="button" v-on:click="options.push(option6)" @click="check">Дальше</button>
    </div>

    <div class="q7" v-if="status==7">
        <h4>7. Как вы чувствуете прибавку массы тела?</h4>
        <ul class="options">
            <input id="7-1" type="radio" name="q7" value="Кардиолог" v-model="option7"><label for="7-1">Плотные отеки на лице утром</label><br>
            <input id="7-2" type="radio" name="q7" value="Кардиолог" v-model="option7"><label for="7-2">Плотные отеки по всему телу вечером</label><br>
            <input id="7-3" type="radio" name="q7" value="Эндокринолог" v-model="option7"><label for="7-3">Отеков нет</label>
        </ul>
        <button type="button" v-on:click="options.push(option7)" @click="check">Дальше</button>
    </div>

    <div class="q8" v-if="status==8">
        <h4>8. Когда вы чувствуете онимение пальцев?</h4>
        <ul class="options">
            <input id="8-1" type="radio" name="q8" value="Невропатолог" v-model="option8"><label for="8-1">Связанно со стрессом</label><br>
            <input id="8-2" type="radio" name="q8" value="Эндокринолог" v-model="option8"><label for="8-2">Независимо от стресса</label>
        </ul>
        <button type="button" v-on:click="options.push(option8)" @click="check">Дальше</button>
    </div>

    <div class="q9" v-if="status==9">
        <h4>9. Когда вы чувствуете запоры?</h4>
        <ul class="options">
            <input id="9-1" type="radio" name="q9" value="Эндокринолог" v-model="option9"><label for="9-1">Постоянно</label><br>
            <input id="9-2" type="radio" name="q9" value="Гастроэнтеролог" v-model="option9"><label for="9-2">С приемом пищи</label>
        </ul>
        <button type="button" v-on:click="options.push(option9)" @click="check">Дальше</button>
    </div>

    <div class="q10" v-if="status==10">
        <h4>10. Когда вы чувствуете выпадение волос?</h4>
        <ul class="options">
            <input id="10-1" type="radio" name="q10" value="Терапевт" v-model="option10"><label for="10-1">С приемом препаратов</label><br>
            <input id="10-2" type="radio" name="q10" value="Эндокринолог" v-model="option10"><label for="10-2">Постоянно</label>
        </ul>
        <button type="button" v-on:click="options.push(option10)" @click="check">Дальше</button>
    </div>

    <div class="q11" v-if="status==11">
        <h4>11. Какое чувство боли в сердце у вас?</h4>
        <ul class="options">
            <input id="11-1" type="radio" name="q11" value="Кардиолог" v-model="option11"><label for="11-1">Учащение ритма</label><br>
            <input id="11-2" type="radio" name="q11" value="Эндокринолог" v-model="option11"><label for="11-2">Замедление ритма</label><br>
            <input id="11-3" type="radio" name="q11" value="Невропатолог" v-model="option11"><label for="11-3">Просто боль</label>
        </ul>
        <button type="button" v-on:click="options.push(option11)" @click="check">Дальше</button>
    </div>

    <div class="q12" v-if="status==12">
        <h4>12. Какие виды судороги у вас?</h4>
        <ul class="options">
            <input id="12-1" type="radio" name="q12" value="Эндокринолог" v-model="option12"><label for="12-1">Мышечные</label><br>
            <input id="12-2" type="radio" name="q12" value="Невропатолог" v-model="option12"><label for="12-2">Эпилептические</label>
        </ul>
        <button type="button" v-on:click="options.push(option12)" @click="check">Дальше</button>
    </div>

    <div class="q13" v-if="status==13">
        <h4>13. Когда вы чувствуете нарушение менструального цикла?</h4>
        <ul class="options">
            <input id="13-1" type="radio" name="q13" value="Гинеколог" v-model="option13"><label for="13-1">С приемом препаратов</label><br>
            <input id="13-2" type="radio" name="q13" value="Эндокринолог" v-model="option13"><label for="13-2">Не связанно, возрастное</label><br>
            <input id="13-3" type="radio" name="q13" value="Гинеколог" v-model="option13"><label for="13-3">Периодически</label>
        </ul>
        <button type="button" v-on:click="options.push(option13)" @click="check">Дальше</button>
    </div>

    <div class="results" v-if="status==14">
        <h4>Вам нужно обратиться к следующим специалистам:</h4>
        <ul v-for="item in [...new Set(options)]">{{item}}</ul>
        <button type="button" @click="reloadPage">Пройти заново</button>
    </div>
    
    <div class="alert">
        <h4 v-if="missing==1">Вы не выбрали ответ!</h4>
    </div>
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
      ttg: '',
      t3: '',
      t4: '',
      tpo: '',
      tg: '',
      psao: '',
      ispsa: '',
      psas: '',
      tireoglubin: '',
      options: [],
      status: 0,
      missing: 0
    }
  },
  methods: {
    reloadPage(){
      window.location.reload();
    },
    check(){
      if (this.options[this.options.length-1]==null){
        this.missing = 1;
      }
      else{
        this.status = this.status + 1;
        this.missing = 0;
      }
    },
    handleFileSelect(event) {
      const file = event.target.files[0]
      const reader = new FileReader();
      reader.onload = () => {
          this.status = this.status+1;
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
                this.ttg = this.ttg + this.extractedText[id]
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
                this.t3 = this.t3 + this.extractedText[id]
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
                this.t4 = this.t4 + this.extractedText[id]
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
                this.tpo = this.tpo + this.extractedText[id]
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
                this.tg = this.tg + this.extractedText[id]
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
                this.psao = this.psao + this.extractedText[id]
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
                this.ispsa = this.ispsa + this.extractedText[id]
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
                this.psas = this.psas + this.extractedText[id]
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
                this.tireoglubin = this.tireoglubin + this.extractedText[id]
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
  /* margin-left: 5%; */
  margin-right: 5%;
}
td, th{
  padding: 5px;
  text-align: center;
}
#app{
  margin-left: 5%;
  margin-top: 2%;
}
.alert{
  color: tomato;
}
</style>
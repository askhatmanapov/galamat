<template>
  <div>
    <div class="upload">
      <input type="file" @change="handleFileSelect" />
    </div>
    <div class="pdf" v-if="ttg">
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
            <td @click="editttg">{{ ttg }} мкМЕ/мл</td>
            <td>0,40 - 3,77</td>
            <td :class="color[0]">{{ comments[0] }}</td>
          </tr>
          <tr>
            <td>Свободный Т3</td>
            <td>{{ t3 }} пг/мл</td>
            <td>2,00 - 4,40</td>
            <td :class="color[1]">{{ comments[1] }}</td>
          </tr>
          <tr>
            <td>Свободный Т4</td>
            <td>{{t4}} нг/дл</td>
            <td>1,00 - 1,70</td>
            <td :class="color[2]">{{ comments[2] }}</td>
          </tr>
          <tr>
            <td>Анти ТПО</td>
            <td>{{tpo}} МЕ/мл</td>
            <td>0 - 34</td>
            <td :class="color[3]">{{ comments[3] }}</td>
          </tr>
          <tr>
            <td>Анти ТГ</td>
            <td>{{tg}} МЕ/мл</td>
            <td>0 - 115</td>
            <td :class="color[4]">{{ comments[4] }}</td>
          </tr>
          <tr>
            <td>ПСА общий</td>
            <td>{{psao}} нг/мл</td>
            <td>0,000 - 2,000</td>
            <td :class="color[5]">{{ comments[5] }}</td>
          </tr>
          <tr>
            <td>Индекс свободного ПСА</td>
            <td>{{ispsa}} %</td>
            <td>выше 15</td>
            <td :class="color[6]">{{ comments[6] }}</td>
          </tr>
          <tr>
            <td>ПСА свободный</td>
            <td>{{psas}} нг/мл</td>
            <td></td>
            <td :class="color[7]">{{ comments[7] }}</td>
          </tr>
          <tr>
            <td>Тиреоглобулин</td>
            <td>{{tireoglubin}} нг/мл</td>
            <td>1,4 - 78</td>
            <td :class="color[8]">{{ comments[8] }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="report">
      <div v-if="report1">
        <h4>У Вас выявлены подозрения на Первичный гипотериоз</h4>
        <p>Ответьте на следующие вопросы чтобы узнать к каким специалистам обратиться! Займет не более 2-х минут.</p>
      </div>
      <div v-if="report2">
        <h4>У Вас выявлены подозрения на Субклинический гипотериоз</h4>
        <p>Ответьте на следующие вопросы чтобы узнать к каким специалистам обратиться! Займет не более 2-х минут.</p>
      </div>
      <div v-if="report3">
        <h4>У Вас выявлены подозрения на Центральный гипотериоз</h4>
        <p>Ответьте на следующие вопросы чтобы узнать к каким специалистам обратиться! Займет не более 2-х минут.</p>
      </div>
      <div v-if="report4">
        <h4>Необходима консультация нейрохирурга</h4>
        <p>Ответьте на следующие вопросы чтобы узнать к каким специалистам обратиться! Займет не более 2-х минут.</p>
      </div>
      <div v-if="report5">
        <h4>Возможна погрешность анализов</h4>
        <p>Необходимо повторно сдать анализ гормонов щитовидной железы</p>
      </div>
    </div>
    <div class="congrats">
      <div v-if="report6">
        <h4>Поздравляем! Все анализы в норме!</h4>
      </div>
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
import Swal from 'sweetalert2';

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
      missing: 0,
      comments: [],
      color: [],
      report1: false,
      report2: false,
      report3: false,
      report4: false,
      report5: false,
      report6: false
    }
  },
  methods: {
    async editttg() {
      const { value: input } = await Swal.fire({
        title: 'Введите новое значение "ТТГ"',
        input: 'text',
        inputValue: this.ttg,
        inputValidator: (value) => {
          if (!value) {
            return 'Введите новое значение "ТТГ"';
          }
        },
      });

      if (input) {
        this.ttg = input;
      }
    },
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
              while (n<4) {
                this.ttg = this.ttg + this.extractedText[id]
                id++;
                n++;
              }

              this.ttgcomment = parseFloat(this.ttg.replace(',', '.'))
              if (0.40 <= this.ttgcomment && this.ttgcomment <= 3.77) {
                this.comments[0] = 'В норме'
                this.color[0] = 'n'
              }
              else if(this.ttgcomment <= 0.40){
                this.comments[0] = 'Ниже нормы'
                this.color[0] = 'bn'
              }
              else if(3.77 <= this.ttgcomment){
                this.comments[0] = 'Выше нормы'
                this.color[0] = 'an'
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]==' ' && this.extractedText[i+1] == 'Т' && this.extractedText[i+2]=='3'){
                  id = i + 8;
                  n = 0;
                }
              }
              while (n<4) {
                this.t3 = this.t3 + this.extractedText[id]
                id++;
                n++;
              }
              this.t3comment = parseFloat(this.t3.replace(',', '.'))
              if (2.00 <= this.t3comment && this.t3comment <= 4.40) {
                this.comments[1] = 'В норме'
                this.color[1] = 'n'
              }
              else if(this.t3comment <= 2.00){
                this.comments[1] = 'Ниже нормы'
                this.color[1] = 'bn'
              }
              else if(4.40 <= this.t3comment){
                this.comments[1] = 'Выше нормы'
                this.color[1] = 'an'
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]==' ' && this.extractedText[i+1] == 'Т' && this.extractedText[i+2]=='4'){
                  id = i + 8;
                  n = 0;
                }
              }
              while (n<4) {
                this.t4 = this.t4 + this.extractedText[id]
                id++;
                n++;
              }
              this.t4comment = parseFloat(this.t4.replace(',', '.'))
              if (1.00 <= this.t4comment && this.t4comment <= 1.70) {
                this.comments[2] = 'В норме'
                this.color[2] = 'n'
              }
              else if(this.t4comment <= 1.00){
                this.comments[2] = 'Ниже нормы'
                this.color[2] = 'bn'
              }
              else if(1.70 <= this.t4comment){
                this.comments[2] = 'Выше нормы'
                this.color[2] = 'an'
              }


              if(this.color[0] == 'bn' && this.color[2] == 'bn'){
                this.report3 = true
                this.status = 1
              }
              else if(this.color[0] == 'bn' && this.color[2] == 'n'){
                this.report4 = true
                this.status = 1
              }
              else if(this.color[0] == 'bn' && this.color[2] == 'an'){
                this.report5 = true;
                this.status = 0
              }
              else if(this.color[0] == 'n' && this.color[2] == 'bn'){
                this.report5 = true;
                this.status = 0
              }
              else if(this.color[0] == 'n' && this.color[2] == 'n'){
                this.status = 0;
                this.report6 = true
              }
              else if(this.color[0] == 'n' && this.color[2] == 'an'){
                this.report5 = true;
                this.status = 0
              }
              else if(this.color[0] == 'an' && this.color[2] == 'bn'){
                this.report1 = true;
                this.status = 1
              }
              else if(this.color[0] == 'an' && this.color[2] == 'n'){
                this.report2 = true;
                this.status = 1
              }
              else if(this.color[0] == 'an' && this.color[2] == 'an'){
                this.report4 = true;
                this.status = 1
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='Т' && this.extractedText[i+1] == 'П' && this.extractedText[i+2]=='О'){
                  id = i + 8;
                  n = 0;
                }
              }
              while (n<3) {
                this.tpo = this.tpo + this.extractedText[id]
                id++;
                n++;
              }
              this.tpocomment = parseFloat(this.tpo.replace(',', '.'))
              if (0 <= this.tpocomment && this.tpocomment <= 34) {
                this.comments[3] = 'В норме'
                this.color[3] = 'n'
              }
              else if(this.tpocomment <= 0){
                this.comments[3] = 'Ниже нормы'
                this.color[3] = 'bn'
              }
              else if(34 <= this.tpocomment){
                this.comments[3] = 'Выше нормы'
                this.color[3] = 'an'
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]==' ' && this.extractedText[i+1] == 'Т' && this.extractedText[i+2]=='Г'){
                  id = i + 8;
                  n = 0;
                }
              }
              while (n<3) {
                this.tg = this.tg + this.extractedText[id]
                id++;
                n++;
              }
              this.tgcomment = parseFloat(this.tg.replace(',', '.'))
              if (0 <= this.tgcomment && this.tgcomment <= 115) {
                this.comments[4] = 'В норме'
                this.color[4] = 'n'
              }
              else if(this.tgcomment <= 0){
                this.comments[4] = 'Ниже нормы'
                this.color[4] = 'bn'
              }
              else if(115 <= this.tgcomment){
                this.comments[4] = 'Выше нормы'
                this.color[4] = 'an'
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='и' && this.extractedText[i+1] == 'й' && this.extractedText[i+3]=='1'){
                  id = i + 7;
                  n = 0;
                }
              }
              while (n<5) {
                this.psao = this.psao + this.extractedText[id]
                id++;
                n++;
              }
              this.psaocomment = parseFloat(this.psao.replace(',', '.'))
              if (0 <= this.psaocomment && this.psaocomment <= 2) {
                this.comments[5] = 'В норме'
                this.color[5] = 'n'
              }
              else if(this.psaocomment <= 0){
                this.comments[5] = 'Ниже нормы'
                this.color[5] = 'bn'
              }
              else if(2 <= this.psaocomment){
                this.comments[5] = 'Выше нормы'
                this.color[5] = 'an'
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i-18]=='И' && this.extractedText[i] == 'П' && this.extractedText[i+1]=='С' && this.extractedText[i+2]=='А'){
                  id = i + 6;
                  n = 0;
                }
              }
              while (n<5) {
                this.ispsa = this.ispsa + this.extractedText[id]
                id++;
                n++;
              }
              this.ispsacomment = parseFloat(this.ispsa.replace(',', '.'))
              if (15 <= this.ispsacomment) {
                this.comments[6] = 'В норме'
                this.color[6] = 'n'
              }
              else if(this.ispsacomment <= 15){
                this.comments[6] = 'Ниже нормы'
                this.color[6] = 'bn'
              }

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i-12]=='П' && this.extractedText[i-11] == 'С' && this.extractedText[i-10]=='А' && this.extractedText[i]=='й'){
                  id = i + 6;
                  n = 0;
                }
              }
              while (n<5) {
                this.psas = this.psas + this.extractedText[id]
                id++;
                n++;
              }
              this.comments[7] = 'В норме'
              this.color[7] = 'n'

              for (let i = 0; i<this.size; i++){
                if(this.extractedText[i]=='б' && this.extractedText[i+1] == 'у' && this.extractedText[i+2]=='л' && this.extractedText[i+3]=='и'){
                  id = i + 10;
                  n = 0;
                }
              }
              while (n<5) {
                this.tireoglubin = this.tireoglubin + this.extractedText[id]
                id++;
                n++;
              }
              this.tireoglubincomment = parseFloat(this.tireoglubin.replace(',', '.'))
              if (1.4 <= this.tireoglubincomment && this.tireoglubincomment <= 78) {
                this.comments[8] = 'В норме'
                this.color[8] = 'n'
              }
              else if(this.tireoglubincomment <= 1.4){
                this.comments[8] = 'Ниже нормы'
                this.color[8] = 'bn'
              }
              else if(78 <= this.tireoglubincomment){
                this.comments[8] = 'Выше нормы'
                this.color[8] = 'an'
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
.n{
  color: green;
}
.bn, .an{
  color: tomato;
}
.report{
  color: lightcoral;
}
.congrats{
  color: green;
}
</style>
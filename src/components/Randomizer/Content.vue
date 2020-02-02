<template>
  <v-content>
    <v-container>
      <v-row
        align="start"
        justify="center"
      >
        <v-col class="text-center">

          <h1>Randomize your input separated by spaces</h1>

          <div class="my-4">
            <v-menu
              v-model="example"
              close-on-click
              close-on-content-click
              offset-y
            >
              <template v-slot:activator="{ on }">
                <v-btn color="success" class="btn-load ma-2" v-on="on">
                  <v-icon left>mdi-folder-download</v-icon>
                  Load example
                  <v-icon right class="pointer-down" :class="{'flip': example}">mdi-chevron-down</v-icon>
                </v-btn>
              </template>
              <v-list>
                <v-list-item
                  v-for="(item, index) in examples"
                  :key="index"
                  @click="loadExample(index)"
                >
                  <v-list-item-title><v-icon left v-if="item.icon">{{ item.icon }}</v-icon> {{ item.label }}</v-list-item-title>
                </v-list-item>
              </v-list>
            </v-menu>

            <v-btn color="warning" class="ma-2" @click="clearInput">
              <v-icon left>mdi-file-remove</v-icon>
              Clear input
            </v-btn>
          </div>

          <v-textarea
            counter
            outlined
            name="input"
            label="Input"
            required
            v-model="randomInput"
          ></v-textarea>

          <v-row align="center" class="mb-4">
            <v-col class="text-center pa-0" cols="3" md="2" lg="1" offset="3" offset-md="4" offset-lg="5">
              <v-text-field
                type="number"
                label="Amount"
                required
                v-model="randomAmount"
              ></v-text-field>
            </v-col>
            <v-col class="text-left" cols="6">
              <v-btn color="primary" @click="onSubmit">
                <v-icon left>mdi-shuffle-variant</v-icon>
                Randomize
              </v-btn>
            </v-col>
          </v-row>

          <v-textarea
            counter
            outlined
            name="result"
            label="Result"
            v-model="randomResult"
          ></v-textarea>

          <div class="my-0">
            <v-btn color="success" class="ma-2" @click="saveOutput">
              <v-icon left>mdi-file-download</v-icon>
              Save result
            </v-btn>
            <v-btn color="warning" class="ma-2" @click="clearOutput">
              <v-icon left>mdi-file-remove</v-icon>
              Clear result
            </v-btn>
          </div>

        </v-col>
      </v-row>
    </v-container>
  </v-content>
</template>

<script>
export default {
  name: 'Content',

  data: () => ({
    isLoading: false,
    example: false,
    examples: {
      'numbersTo6': {
        label: 'Numbers from 1 to 6 (dice throw)',
        icon: 'mdi-dice-6',
        input: Array.from(Array(6), (_, i) => i + 1).join(' '),
        amount: 1
      },
      'numbersTo100': {
        label: 'Numbers from 1 to 100 (chance)',
        icon: 'mdi-percent',
        input: Array.from(Array(100), (_, i) => i + 1).join(' '),
        amount: 1
      },
      'maleNames': {
        label: 'Male names',
        icon: 'mdi-gender-male',
        input: ["PIOTR","KRZYSZTOF","ANDRZEJ","TOMASZ","JAN","PAWEŁ","MICHAŁ","MARCIN","STANISŁAW","JAKUB","ADAM","MAREK","ŁUKASZ","GRZEGORZ","MATEUSZ","WOJCIECH","MARIUSZ","DARIUSZ","ZBIGNIEW","JERZY","MACIEJ","RAFAŁ","ROBERT","JÓZEF","KAMIL","JACEK","TADEUSZ","DAWID","RYSZARD","SZYMON","KACPER","JANUSZ","BARTOSZ","JAROSŁAW","MIROSŁAW","SŁAWOMIR","HENRYK","ARTUR","SEBASTIAN","DAMIAN","PATRYK","KAZIMIERZ","PRZEMYSŁAW","DANIEL","KAROL","ROMAN","MARIAN","WIESŁAW","ANTONI","FILIP","ADRIAN","ARKADIUSZ","ALEKSANDER","DOMINIK","BARTŁOMIEJ","LESZEK","FRANCISZEK","WALDEMAR","MIKOŁAJ","ZDZISŁAW","KRYSTIAN","RADOSŁAW","WIKTOR","BOGDAN","EDWARD","MIECZYSŁAW","KONRAD","WŁADYSŁAW","HUBERT","CZESŁAW","IGOR","EUGENIUSZ","OSKAR","STEFAN","BOGUSŁAW","ZYGMUNT","IRENEUSZ","MARCEL","WITOLD","MAKSYMILIAN","SYLWESTER","MIŁOSZ","WŁODZIMIERZ","ZENON","ALAN","OLIWIER","CEZARY","NIKODEM","NORBERT","LEON","GABRIEL","JULIAN","BŁAŻEJ","OLEKSANDR","FABIAN","BRONISŁAW","IGNACY","EMIL","ERYK","WACŁAW","TYMOTEUSZ","LECH","BOLESŁAW","TYMON","BERNARD","EDMUND","SERHII","VOLODYMYR","ANDRII","REMIGIUSZ","KSAWERY","NATAN","LUCJAN","OLAF","ROMUALD","BORYS","KAJETAN","SZCZEPAN","ALBERT","SEWERYN","GRACJAN","ALFRED","KUBA","DMYTRO","TOBIASZ","IVAN","LUDWIK","JOACHIM","MYKOLA","LESŁAW","BOGUMIŁ","VASYL","GERARD","VITALII","ERNEST","MAKSYM","IHOR","BRUNO","FELIKS","KORNEL","OLIVIER","JĘDRZEJ","YURII","ALOJZY","VIKTOR","OLEH","ALEXANDER","ALEX","MYKHAILO","BOHDAN","JULIUSZ","LEONARD","KLAUDIUSZ","DAVID","BENEDYKT","ALEKS","DORIAN","RAJMUND","TEODOR","CYPRIAN","MARTIN","OLIVER","OLEKSII","VLADYSLAV","RUDOLF","NATANIEL","KONSTANTY","RUSLAN","DENIS","HIERONIM","BRAJAN","GUSTAW","MICHAEL","WINCENTY","SAMUEL","ZYGFRYD","MARCELI","PAVLO","FLORIAN","ERWIN","MIESZKO","KEVIN","FRYDERYK","ARIEL","DENYS","ANATOLII","IWO","VADYM","MILAN","ARTEM","ALEKSY","PETRO","BENIAMIN","ROLAND","SERGIUSZ","WALENTY","YAROSLAV","KORDIAN","ADOLF","OLEG","AUGUSTYN","YEVHEN","AMADEUSZ","TARAS","ANTON","BRUNON","JEREMI","OLGIERD","LEOPOLD","VALERII","PATRICK","ALIAKSANDR","EMILIAN","KEWIN","MAXIMILIAN","HERBERT","SERGII","ALFONS","ALBIN","ANDREI","YEVHENII","THOMAS","ARNOLD","OLIWER","OSCAR","WILHELM","NICOLAS","VICTOR","VIACHESLAV","STANISLAV","ANATOL","MAURYCY","HELMUT","WALTER","GINTER","OKTAWIAN","MANFRED","PETER","EDWIN","TEOFIL","SIARHEI","ANDRIY","KOSTIANTYN","EMANUEL","KORNELIUSZ","WERNER","JEREMIASZ","JONASZ","WALERIAN","STEPAN","KRYSPIN","JONATAN","CHRISTIAN","LECHOSŁAW","KASPER","NATHAN","ZIEMOWIT","VALENTYN","DZMITRY","XAVIER","BENJAMIN","NIKOLAS","MARK","LEO","KLEMENS","FERDYNAND","JACOB","PATRYCJUSZ","TYTUS","MIRON","LONGIN","NAZAR","MAX","ARON","EDUARD","IURII","PAVEL","KSAWIER","PAUL","HORST","YURIY","LEONID","ILLIA","WAWRZYNIEC","HUGO","JACENTY","MATTHEW","PHILIP","ANDREAS","CHRISTOPHER","ELIASZ","IZYDOR","DIONIZY","ŁUCJAN","EWALD","GERHARD","ELIGIUSZ","JOHN","ANTHONY","MARCO"].join(' '),
        amount: 2
      },
      'femaleNames': {
        label: 'Female names',
        icon: 'mdi-gender-female',
        input: ["ANNA","MARIA","KATARZYNA","MAŁGORZATA","AGNIESZKA","BARBARA","EWA","KRYSTYNA","ELŻBIETA","MAGDALENA","JOANNA","ZOFIA","ALEKSANDRA","MONIKA","TERESA","DANUTA","NATALIA","KAROLINA","MARTA","BEATA","JULIA","DOROTA","JANINA","JADWIGA","HALINA","JOLANTA","IRENA","ALICJA","GRAŻYNA","IWONA","PAULINA","JUSTYNA","BOŻENA","ZUZANNA","URSZULA","WIKTORIA","RENATA","HELENA","SYLWIA","AGATA","HANNA","PATRYCJA","IZABELA","MAJA","ANETA","EMILIA","WERONIKA","EWELINA","MARIANNA","OLIWIA","STANISŁAWA","MARTYNA","KLAUDIA","MARZENA","GABRIELA","DOMINIKA","KINGA","EDYTA","WIESŁAWA","KAMILA","LENA","WANDA","AMELIA","LUCYNA","ALINA","LIDIA","MARIOLA","MIROSŁAWA","WIOLETTA","NIKOLA","DARIA","KAZIMIERA","MILENA","GENOWEFA","ANGELIKA","BOGUMIŁA","JÓZEFA","STEFANIA","HENRYKA","OLGA","SANDRA","ANTONINA","ILONA","SABINA","REGINA","BOGUSŁAWA","LAURA","WŁADYSŁAWA","MARLENA","CZESŁAWA","MICHALINA","ANITA","ŁUCJA","NADIA","KORNELIA","ANIELA","JAGODA","WIOLETA","IGA","NINA","LILIANA","CECYLIA","MARCELINA","ROKSANA","DAGMARA","ADRIANNA","KARINA","LEOKADIA","ZDZISŁAWA","MALWINA","POLA","BRONISŁAWA","ŻANETA","EUGENIA","SARA","ELIZA","BERNADETA","RÓŻA","DANIELA","JULITA","ALDONA","KAJA","ROZALIA","VIOLETTA","ANASTAZJA","CELINA","MAGDA","HONORATA","BLANKA","KLARA","GERTRUDA","DIANA","ADRIANA","MIECZYSŁAWA","BRYGIDA","PAULA","LILIANNA","IZABELLA","BOŻENNA","MATYLDA","TETIANA","NATALIIA","IRYNA","OLENA","ELWIRA","OKSANA","MARZANNA","AURELIA","MARIKA","SONIA","ANDŻELIKA","KALINA","FRANCISZKA","ARLETA","ADELA","ALFREDA","OLIVIA","SVITLANA","LUIZA","OLHA","JUDYTA","NELA","NATASZA","NICOLA","NICOLE","LUDWIKA","TATIANA","JOWITA","ROMANA","MARIIA","WACŁAWA","ELEONORA","APOLONIA","VICTORIA","JULIANNA","VANESSA","ZENONA","MARZENNA","YULIIA","JESSICA","INGA","HILDEGARDA","TAMARA","ESTERA","WALENTYNA","BERNADETTA","ZENOBIA","FELICJA","VIKTORIIA","KATERYNA","ADA","LUDMIŁA","DONATA","MELANIA","ROMUALDA","LIUDMYLA","LIWIA","ANETTA","ANASTASIIA","PELAGIA","GIZELA","OTYLIA","HALYNA","IRMINA","AMANDA","ERYKA","BIANKA","WALERIA","SŁAWOMIRA","TEODOZJA","MARYNA","BERNARDA","LONGINA","TEODORA","LILLA","RITA","ELENA","RYSZARDA","LILIA","VALENTYNA","ADELAJDA","MIA","MAYA","IDA","LARYSA","NADIIA","ALBINA","MIRELA","KAMILLA","INNA","FELIKSA","EMMA","LUCJA","LEONARDA","OLIMPIA","FAUSTYNA","SAMANTA","ZYTA","MARYLA","LILA","MARCELA","OLEKSANDRA","ALEXANDRA","ANGELINA","LIUBOV","BOLESŁAWA","ALLA","KSENIA","PAMELA","OKTAWIA","VIKTORIA","KONSTANCJA","WANESSA","SALOMEA","YANA","BOGNA","SOFIA","NADZIEJA","ELFRYDA","EDWARDA","MIRELLA","KRZYSZTOFA","CLAUDIA","INEZ","NOEMI","EDELTRAUDA","LILIIA","ZOJA","GAJA","MARIYA","VERONIKA","ALONA","OLA","MARIANA","NEL","JAGNA","SOPHIE","STELLA","MICHELLE","WIERA","ANGELA","IVANNA","NATALIYA","SARAH","EMILY","RUTA","ARLETTA","IZA","SOFIIA","KHRYSTYNA","HELGA","MIRIAM","KLEMENTYNA","TOLA","KLAUDYNA","MANUELA","INES","GERDA","ROMA","MARIETTA"].join(' '),
        amount: 2
      }
    },
    randomAmount: '',
    randomInput: '',
    randomResult: ''
  }),

  mounted: function () {
    this.loadFromStorage('randomAmount');
    this.loadFromStorage('randomInput');
    this.loadFromStorage('randomResult');
  },
  watch: {
    randomAmount: function () {
      this.saveToStorage('randomAmount');
    },
    randomInput: function () {
      this.saveToStorage('randomInput');
    },
    randomResult: function () {
      this.saveToStorage('randomResult');
    }
  },
  methods: {
    loadExample: function(type) {
      if (this.examples[type]) {
        if (this.examples[type].input) {
          this.randomInput = this.examples[type].input;
        }
        if (this.examples[type].amount) {
          this.randomAmount = this.examples[type].amount;
        }
        // clear result
        // this.randomResult = '';
        if (this.randomAmount) {
          this.$emit('alert', `Example ${this.examples[type].label} has been loaded.`, 'success');
        }
      } else {
        return this.$emit('alert', 'The example does not exist.', 'error');
      }
    },
    getRandomInt: function (min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    onSubmit: function () {
      this.isLoading = true;
      let random = this.randomInput;
      // replace new lines with spaces
      random = random.replace(/(?:\r\n|\r|\n)/g, ' ');
      // remove dots and comas
      random = random.replace(/(?:\.|,)/g, '');
      // validate empty input
      if ( ! random.length) {
        return this.$emit('alert', 'The input is empty.', 'error');
      }
      // validate incorrect abmount
      if (this.randomAmount <= 0) {
        return this.$emit('alert', 'The amount is incorrect.', 'error');
      }
      // convert string to array
      const randomArray = random.split(' ');
      let output = '';
      // randomize input and glue it as a result
      for (let i = 0, len = parseInt(this.randomAmount); i < len; i++) {
        const randomIndex = this.getRandomInt(0, randomArray.length - 1);
        if (randomArray[randomIndex]) {
          output += randomArray[randomIndex] + ' ';
        }
      }
      this.randomResult = output.trim();
      this.isLoading = false;
      if (this.randomResult) {
        this.$emit('alert', 'The input have been randomized.', 'success');
      } else {
        return this.$emit('alert', 'The result is empty.', 'error');
      }
    },
    saveToStorage: function (key) {
      if (this[key] !== undefined) {
        const parsed = JSON.stringify(this[key]);
        localStorage.setItem(key, parsed);
      }
    },
    loadFromStorage: function (key) {
      if (localStorage.getItem(key)) {
        this[key] = JSON.parse(localStorage.getItem(key));
      }
    },
    clearStorage: function () {
      localStorage.clear();
      location.reload();
    },
    clearInput: function () {
      if (this.randomInput) {
        this.randomInput = '';
        this.$emit('alert', 'The input has been cleared.', 'success');
      } else {
        this.$emit('alert', 'The input is already empty.', 'error');
      }
    },
    clearOutput: function () {
      if (this.randomResult) {
        this.randomResult = '';
        this.$emit('alert', 'The output has been cleared.', 'success');
      } else {
        this.$emit('alert', 'The output is already empty.', 'error');
      }
    },
    saveOutput: function () {
      if (this.randomResult) {
        this.saveFile(this.randomResult, 'random-output.txt');
        this.$emit('alert', 'The output has been saved.', 'success');
      } else {
        this.$emit('alert', 'The output is empty and cannot be saved.', 'error');
      }
    },
    saveFile: function (textData, fileName) {
      const blob = new Blob([textData], {type: 'text/plain'});
      const e = document.createEvent('MouseEvents');
      const a = document.createElement('a');
      a.download = fileName;
      a.href = window.URL.createObjectURL(blob);
      a.dataset.downloadurl = ['text/plain', a.download, a.href].join(':');
      e.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
      a.dispatchEvent(e);
    }
  }
}
</script>

<style lang="scss" scoped>
.pointer-down {
  transition: 0.3s;
  transform: rotate(0deg);

  &.flip {
    transform: rotate(180deg);
  }
}
</style>

<template>
  
<div id="app">
  
<p v-if="blad">UWAGA! FAŁSZYWE DANE, BRAK DANYCH LUB NIEPRAWIDŁOWE!</p>

<div id="zdrowie" v-if="etap0">
<h1>Podaj dane o sobie</h1>

<p>Podaj płeć</p>
<input type="radio" id="mezczyna" name="plec" v-model="radioBoxOption" value=0.7>
<label for="mezczyzna">Mężczyzna</label>
<input type="radio" id="kobieta" name="plec" v-model="radioBoxOption" value=0.6>
<label for="kobieta">Kobieta</label>
<br>
<label for="wiek">Wprowadź wiek:</label>
<input type="number" id="wiek" name="wiek" v-model.number="zdrowie.wiek">
<br>
<label for="wzrost">Wprowadź wzrost:</label>
<input type="number" id="wzrost" name="wzrost" v-model.number="zdrowie.wzrost">
<br>
<label for="waga">Wprowadź wage:</label>
<input type="number" id="waga" name="waga" v-model.number="zdrowie.waga">
<br><br>
<button class="btn btn-primary"
        @click.prevent="checkForm_zdrowie">Dalej!
</button>

</div>

<div id="zdrowie" v-if="etap1">
        <h1>Dane o spożyciu:</h1>
        <p>piwa (5%) porcji po 250ml:</p>
        <input type="number" id="piwa" name="piwa" v-model.number="alkohol.piwa">
        <p>wódki (40%) porcji po 30ml:</p>
        <input type="number" id="wodka" name="woda" v-model.number="alkohol.wodka">
        <p>wina (12%) porcji po 100ml: </p>
        <input type="number" id="piwa" name="wina" v-model.number="alkohol.wino">
        <p>Ile godzin temu piłeś?</p>
        <input type="number" id="godziny" name="godziny" v-model.number="alkohol.godzina">
        <br><br>
        <button class="btn btn-primary"
                @click.prevent="cofnij_1">Cofnij!
        </button>
        <button class="btn btn-primary"
                @click.prevent="checkForm_spozycie">Dalej!
        </button>
</div>

<div id="zdrowie" v-if="etap2">
        <h1>Wynik</h1>

        <p>Podsumowanie: </p>
        <p>Wiek: {{zdrowie.wiek}}</p>
        <p>Waga: {{zdrowie.waga}}</p>
        <p>Wzrost: {{zdrowie.wzrost}}</p>
        <p>Wypite piwa: {{alkohol.piwa}}</p>
        <p>Wypite szklanki wina: {{alkohol.wino}}</p>
        <p>Wypite kieliszki wódki: {{alkohol.wodka}}</p>
        <p>Ilość spożytego alkoholu (gram): {{gramy_alkoholu}}</p>
        <p>Ile godzin temu piłeś: {{alkohol.godzina}}</p>
        <p>Wynik końcowy: {{promile}}%%</p>
        <br>
        <button class="btn btn-primary"
                @click.prevent="cofnij_2">Cofnij!
        </button>
</div>


  </div>

</template>

<script>
    export default {
        data() {
            return {
                zdrowie: {
                    wiek: null,
                    waga: null,
                    value: '2',
                    wzrost: null,
                },
                alkohol: {
                  piwa: null,
                  wodka: null,
                  wino: null,
                  godzina: null,
                },
                etap0: true,
                etap1: false,
                etap2: false,
                radioBoxOption: null,
                blad: null,
                gramy_alkoholu: null,
                promile: null,
            }
        },
          methods: {
            checkForm_zdrowie(){
              if (this.zdrowie.wiek >= 18 && this.zdrowie.wiek < 120 &&
                  this.zdrowie.waga > 20 && this.zdrowie.waga < 300 &&
                  this.zdrowie.wzrost > 30 && this.zdrowie.wzrost < 300 &&
                  this.radioBoxOption != null){
                  this.etap0 = false;
                  this.etap1 = true;
              } else {
                this.blad = 1;
              }
            },
            checkForm_spozycie(){
              if(this.alkohol.piwa >= 0 && this.alkohol.piwa < 30 &&
                 this.alkohol.wodka >= 0 && this.alkohol.wino >= 0){
                   this.etap2 = 1;
                   this.etap1 = 0;
                   this.oblicz();
                 } else {
                   this.blad = 1;
                 }
            },
            cofnij_1(){
              this.etap0 = 1;
              this.etap1 = 0;
              this.etap2 = 0;
            },
            cofnij_2(){
              this.etap0 = 0;
              this.etap1 = 1;
              this.etap2 = 0;
            },
            oblicz(){
              this.gramy_alkoholu = (this.alkohol.piwa * 20) + (this.alkohol.wodka * 10) + (this.alkohol.wino * 10);
              this.promile = (this.gramy_alkoholu / (this.radioBoxOption * this.zdrowie.waga))-0.13*this.alkohol.godzina;
              this.promile = this.promile.toFixed(2);
            },
        }
    }
</script>

<style>
#app {
margin: auto;
}
</style>

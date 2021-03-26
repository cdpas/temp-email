<template>
  <v-container>
    <v-row class="control">
      <h1 class="top">Temp Email</h1>

      <div class="mail">
        <p class="mailBox"
        >
          <input 
          type="text" 
          id="copy-mail" 
          :value="mail + domain" 
          class="inputMail"
          >
          <v-icon
          @click="clipCopy()"
          class="copyIcon"
          >mdi-content-copy</v-icon></p>
      </div>
      <div class="genSave">
        <v-btn 
        style="margin-right: 20px"
        color="green"
        @click="genRandom()"
        >New</v-btn>
        <v-btn 
        style="margin-left: 20px"
        color="blue"
        @click="saveMail()"
        >Save</v-btn>
      </div>
      <div 
      v-show="this.savedMails.length > 0"
      class="saved">
        <div style="float: left;">
          <v-select
          v-model="selectedMail"
          :items="savedMails"
          :label="savedMails[0]"
          style="max-width: 45vw"
          solo
          dense
        ></v-select>
        </div>
        <div style="float: right; margin-top: 5px">
          <v-btn
          @click="loadMail"
          color="yellow"
          style="margin-right: 10px"
          >Load</v-btn>
           <v-btn
          @click="deleteMail"
          color="red"
          >Delete</v-btn>
        </div>
      </div>
    </v-row>
    <v-row class="frame">
      <iframe :src="getMail()" frameborder="0"
        style="width: 99vw; height: 100%;margin-left: auto; margin-right:auto"
        >
        </iframe>
    </v-row>
    <v-snackbar
      v-model="snackbar"
      timeout="3000"
    >
      {{ snackText }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="pink"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>
<style scoped>
.control {
  height: 35vh;
  border: 2px solid black;
  border-radius: 5px;
}
.frame {
 height: 65vh;

 border: 2px solid black;
 border-radius: 5px;
}
.top {
  width: 100vw;
  height: fit-content;
  text-align: center;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
.mail {
  height: fit-content;
  width: 100vw;
  text-align: center;
}
input:focus{
    outline: none;
}
.mailBox {
  border: 1px solid black; 
  border-radius: 10px; 
  max-width: 90vw; 
  margin-left: auto; 
  margin-right: auto; 
  padding: 10px;
  background-color: white;
}
.inputMail {
  text-align: center; 
  width:75vw; 
  border: 0!important; 
  font-size: 18px; 
  font-weight: bold;
}
.v-text-field .v-text-field--solo .v-input__control {
  min-height: 40px!important;
  max-height: 40px!important;
}
.v-btn:not(.v-btn--round).v-size--default {
    height: 28px;
    min-width: 64px;
    padding: 0 10px;
}
button {
  font-size: 11px!important;
}
.copyIcon {
  font-size:18px!important; 
  color: red!important;
}
.genSave {
  width: 90vw; 
  text-align: center; 
  margin-left: auto; 
  margin-right: auto; 
  margin-bottom: 12px;
}
.saved {
  width: 90vw; 
  text-align:center; 
  margin-left: auto; 
  margin-right: auto;
}
</style>
<script>
  import wordList from './list.json';
  export default {
    name: 'Home',

    data: () => ({
     mail: "",
     domain: "@maildrop.cc",
     savedMails: [],
     selectedMail: '',
     snackbar: false,
     snackText: ''
      
      
    }),
    methods: {
      genRandom() {
      var random1 = Math.floor(Math.random() * (2047 - 0) + 0);
      var random2 = Math.floor(Math.random() * (2047 - 0) + 0);
      this.mail = wordList[random1] + wordList[random2] + Math.floor(Math.random() * (99 - 0) + 0);
      this.snackbar = true;
      this.snackText = "Random Mail Generated!";
      },
      getMail() {
          return "https://maildrop.cc/inbox/" + this.mail;
      },
      saveMail() {
       this.savedMails.push(this.mail);
       localStorage.setItem("tempMailList", JSON.stringify(this.savedMails));
       this.snackbar = true;
      this.snackText = "Mail saved!";
      },
      clipCopy() {
        let value = document.querySelector('#copy-mail');
        value.setAttribute('type', 'text');
        value.select();
        value.setSelectionRange(0, 99999);
        document.execCommand("copy");
        this.snackbar = true;
        this.snackText = "Mail copied!";
      },
      checkTemp() {
        if (localStorage.getItem("tempMailList")) {
         this.savedMails = JSON.parse(localStorage.getItem("tempMailList")); 
         this.selectedMail = this.savedMails[0];
        } else {
          this.savedMails = new Array(0);
          console.log(typeof(this.savedMails));
          localStorage.setItem("tempMailList", this.savedMails)
        }
        
      },
      loadMail() {
        if (this.mail != this.selectedMail) {
          this.mail = this.selectedMail;
          this.snackbar = true;
          this.snackText = "Mail loaded!";
        }
      },
      deleteMail() {
        if (this.selectedMail.length < 1) {
          this.selectedMail = this.mail;
        }
        var index = this.savedMails.indexOf(this.selectedMail);
        if (index > -1) {
          this.savedMails.splice(index, 1);
        }
        localStorage.setItem("tempMailList", JSON.stringify(this.savedMails));
        this.snackbar = true;
        this.snackText = "Mail deleted!";
      }
    },
    mounted() {
      this.checkTemp();
      this.savedMails.length > 0 ? this.mail = this.savedMails[0] : this.genRandom();
    }
  }
</script>

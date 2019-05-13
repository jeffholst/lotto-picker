<template>
  <v-app>
    <v-container fluid>
      <v-layout align-center column fill-height>
        <v-flex xs12>
          <v-select
            :items="games"
            label="Select Game"
            item-value="value"
            item-text="text"
            @change="onChange"
            outline
          ></v-select>
        </v-flex>
        <v-flex>
          <v-card>
            <v-card-title primary-title>
              <div>
                <div>
                  <img :src="logoURL">
                </div>
                <div>
                  <span v-html="rawHtml"></span>
                </div>
                <div v-if="lastSelectedGame">
                  <v-btn @click="onChange(lastSelectedGame)" round color="primary" dark>draw Again</v-btn>
                  <v-switch v-model="sort" :label="`Sort`" @change="generateHTML"></v-switch>
                </div>
              </div>
            </v-card-title>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      lastSelectedGame: 0,
      logoURL: require("./assets/texas-lottery-logo.jpg"),
      sort: false,
      Balls: [],
      BallCount: 0,
      BonusBallHTML: '',
      HasBonusBall: false,
      games: [
        { text: "Powerball", value: "1" },
        { text: "Mega Millions", value: "2" },
        { text: "Lotto Texas", value: "3" },
        { text: "Texas Two Step", value: "4" },
        { text: "All or Nothing", value: "5" },
        { text: "Pick 3", value: "6" },
        { text: "Daily 4", value: "7" },
        { text: "Cash Five", value: "8" }
      ],
      rawHtml: ""
    };
  },
  methods: {
    onChange(e) {
      let GameName;
      let BallMinNumber;
      let BallMaxNumber;
      let BonusBall;
      let BonusBallMinNumber;
      let BonusBallMaxNumber;
      let AllowDupes;

      this.Balls = [];

      this.lastSelectedGame = e;
      this.BonusBallHTML = ''

      switch (e) {
        case "1":
          GameName = "Powerball";
          this.logoURL = require("./assets/powerball.png");
          this.BallCount = 5;
          BallMinNumber = 1;
          BallMaxNumber = 69;
          this.HasBonusBall = true;
          BonusBallMinNumber = 1;
          BonusBallMaxNumber = 26;
          AllowDupes = false;
          BonusBall = this.getBonusBall(BonusBallMinNumber, BonusBallMaxNumber)
          this.BonusBallHTML = "<h3>Powerball</h3>"
          this.BonusBallHTML += `<ul class="lotto">`;
          this.BonusBallHTML += `<li class="powerball">${BonusBall}</li>`;
          this.BonusBallHTML += `</ul>`;
          break;
        case "2":
          GameName = "Mega Millions";
          this.logoURL = require("./assets/mega_millions.png");
          this.BallCount = 5;
          BallMinNumber = 1;
          BallMaxNumber = 70;
          this.HasBonusBall = true;
          BonusBallMinNumber = 1;
          BonusBallMaxNumber = 25;
          AllowDupes = false;
          BonusBall = this.getBonusBall(BonusBallMinNumber, BonusBallMaxNumber)
          this.BonusBallHTML = "<h3>Mega Ball</h3>"
          this.BonusBallHTML += `<ul class="lotto">`;
          this.BonusBallHTML += `<li class="megaball">${BonusBall}</li>`;
          this.BonusBallHTML += `</ul>`;
          break;
        case "3":
          GameName = "Lotto Texas";
          this.logoURL = require("./assets/lotto_texas.png");
          this.BallCount = 6;
          BallMinNumber = 1;
          BallMaxNumber = 54;
          this.HasBonusBall = false;
          BonusBallMinNumber = 0;
          BonusBallMaxNumber = 0;
          AllowDupes = false;
          break;
        case "4":
          GameName = "Texas Two Step";
          this.logoURL = require("./assets/texas_two_step.png");
          this.BallCount = 4;
          BallMinNumber = 1;
          BallMaxNumber = 35;
          this.HasBonusBall = true;
          BonusBallMinNumber = 1;
          BonusBallMaxNumber = 35;
          AllowDupes = false;
          BonusBall = this.getBonusBall(BonusBallMinNumber, BonusBallMaxNumber)
          this.BonusBallHTML = "<h3>Bonus Ball</h3>"
          this.BonusBallHTML += `<ul class="lotto">`;
          this.BonusBallHTML += `<li class="bonusball">${BonusBall}</li>`;
          this.BonusBallHTML += `</ul>`;
          break;
        case "5":
          GameName = "All or Nothing";
          this.logoURL = require("./assets/all_or_nothing.png");
          this.BallCount = 12;
          BallMinNumber = 1;
          BallMaxNumber = 24;
          this.HasBonusBall = false;
          BonusBallMinNumber = 0;
          BonusBallMaxNumber = 0;
          AllowDupes = false;
          break;
        case "6":
          GameName = "Pick 3";
          this.logoURL = require("./assets/pick3.png");
          this.BallCount = 3;
          BallMinNumber = 0;
          BallMaxNumber = 9;
          this.HasBonusBall = true;
          BonusBallMinNumber = 0;
          BonusBallMaxNumber = 9;
          AllowDupes = true;
          BonusBall = this.getBonusBall(BonusBallMinNumber, BonusBallMaxNumber)
          this.BonusBallHTML = "<h3>FIREBALL</h3>"
          this.BonusBallHTML += `<ul class="lotto">`;
          this.BonusBallHTML += `<li class="fireball">${BonusBall}</li>`;
          this.BonusBallHTML += `</ul>`;
          break;
        case "7":
          GameName = "Daily 4";
          this.logoURL = require("./assets/daily4.png");
          this.BallCount = 4;
          BallMinNumber = 0;
          BallMaxNumber = 9;
          this.HasBonusBall = true;
          BonusBallMinNumber = 0;
          BonusBallMaxNumber = 9;
          AllowDupes = true;
          BonusBall = this.getBonusBall(BonusBallMinNumber, BonusBallMaxNumber)
          this.BonusBallHTML = "<h3>FIREBALL</h3>"
          this.BonusBallHTML += `<ul class="lotto">`;
          this.BonusBallHTML += `<li class="fireball">${BonusBall}</li>`;
          this.BonusBallHTML += `</ul>`;
          break;
        case "8":
          GameName = "Cash Five";
          this.logoURL = require("./assets/cash_five.png");
          this.BallCount = 5;
          BallMinNumber = 1;
          BallMaxNumber = 37;
          this.HasBonusBall = false;
          BonusBallMinNumber = 0;
          BonusBallMaxNumber = 0;
          AllowDupes = false;
          break;
      }

      if (AllowDupes) {
        for (let loop = 0; loop < this.BallCount; loop++) {
          let r =
            Math.floor(Math.random() * (BallMaxNumber - BallMinNumber + 1)) +
            BallMinNumber;
          this.Balls.push(r);
        }
      } else {
        let Counter = BallMinNumber;
        let TempBalls = [];
        for (let loop = 0; loop <= BallMaxNumber - BallMinNumber; loop++) {
          TempBalls.push(Counter++);
        }
        for (let loop = 0; loop <= BallMaxNumber - BallMinNumber; loop++) {
          let Temp = TempBalls[loop];
          let index = Math.floor(
            Math.random() * (BallMaxNumber - BallMinNumber)
          );
          TempBalls[loop] = TempBalls[index];
          TempBalls[index] = Temp;
        }
        for (let loop = 0; loop < this.BallCount; loop++) {
          this.Balls.push(TempBalls[loop]);
        }
      }

      this.generateHTML()
    },
    generateHTML(){
      this.rawHtml = "";
      this.rawHtml += `<ul class="lotto">`;

      let TempBalls = this.Balls.slice(0)


      if (this.sort){
        TempBalls.sort(function(a, b){return a - b})
      }

      for (let loop = 0; loop < this.BallCount; loop++) {
        this.rawHtml += `<li>${TempBalls[loop]}</li>`
      }

      this.rawHtml += `</ul>`;

      if (this.HasBonusBall){
        this.rawHtml += this.BonusBallHTML
      }
    },
    getBonusBall(min, max){
      return(Math.floor(
            Math.random() * (max - min + 1)
          ) + min)
    }
  }
};
</script>

<style>
ul.lotto {
  list-style-type: none;
}

ul.lotto li {
  display: inline-block;
  color: #ff6600;
  font-family: "Roboto";
  font-size: 150%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 175%;
  width: 37px;
  padding-left: 0px;
  background-image: url(./assets/ball_background.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
}

ul.lotto li.powerball {
  display: inline-block;
  color: #ffffff;
  font-family: "Roboto";
  font-size: 150%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 175%;
  width: 37px;
  padding-left: 0px;
  background-image: url(./assets/powerball_background.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
}

ul.lotto li.megaball {
  display: inline-block;
  color: #15084e;
  font-family: "Roboto";
  font-size: 150%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 175%;
  width: 37px;
  padding-left: 0px;
  background-image: url(./assets/mega_millions_background.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
}

ul.lotto li.bonusball {
  display: inline-block;
  color: #ffffff;
  font-family: "Roboto";
  font-size: 150%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 175%;
  width: 37px;
  padding-left: 0px;
  background-image: url(./assets/powerball_background.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
}

ul.lotto li.fireball {
  display: inline-block;
  color: #ffffff;
  font-family: "Roboto";
  font-size: 150%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 250%;
  width: 63px;
  padding-left: 15px;
  background-image: url(./assets/fireball_background.png);
  background-repeat: no-repeat;
  text-align: left;
  margin-right: 10px;
}
</style>

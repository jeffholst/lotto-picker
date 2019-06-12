<template>
  <v-app>
    <v-container grid-list-md text-xs-center>
      <v-layout row align-center column>
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
        <v-flex v-if="lastSelectedGame">
          <div>
            <div>
              <span v-html="rawHtml"></span>
            </div>
            <p>&nbsp;</p>
            <div >
              <v-btn @click="onChange(lastSelectedGame)" round color="primary" dark>draw Again</v-btn>
              <v-switch v-model="sort" :label="`Sort`" @change="generateHTML" class="justify-center"></v-switch>
            </div>
          </div>
        </v-flex>
        <v-flex v-else>
          <img src="./assets/LottoPicker.png">
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
      let GameName; // eslint-disable-line no-unused-vars
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
        this.rawHtml += "<p>&nbsp;</p>"
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
  color: #ffffff;
  font-family: "Roboto";
  font-size: 225%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 160%;
  width: 50px;
  padding-left: 0px;
  background-image: url(./assets/ball_purple.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
  margin-top: 10px;
}

ul.lotto li.powerball {
  display: inline-block;
  color: #ffffff;
  font-family: "Roboto";
  font-size: 225%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 160%;
  width: 50px;
  padding-left: 0px;
  background-image: url(./assets/ball_red.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
  margin-top: 10px;
}

ul.lotto li.megaball {
  display: inline-block;
  color: #f80404;
  font-family: "Roboto";
  font-size: 225%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 160%;
  width: 50px;
  padding-left: 0px;
  background-image: url(./assets/ball_yellow.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
  margin-top: 10px;
}

ul.lotto li.bonusball {
  display: inline-block;
  color: #f4f804;
  font-family: "Roboto";
  font-size: 225%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 160%;
  width: 50px;
  padding-left: 0px;
  background-image: url(./assets/ball_blue.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
  margin-top: 10px;
}

ul.lotto li.fireball {
  display: inline-block;
  color: #f4f804;
  font-family: "Roboto";
  font-size: 225%;
  font-style: normal;
  font-weight: bold;
  height: 50px;
  line-height: 160%;
  width: 50px;
  padding-left: 0px;
  background-image: url(./assets/ball_red.png);
  background-repeat: no-repeat;
  text-align: center;
  margin-right: 10px;
  margin-top: 10px;
}
</style>

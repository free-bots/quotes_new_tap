<template>
  <div>
    <div>
      <Clock
        class="clock"
        v-bind:showSec="this.showSec"
        v-bind:hour="this.hour"
        v-bind:min="this.min"
        v-bind:sec="this.sec"
      />
      <div class="quote-container">
        <p class="quote">{{ this.quote.text }}</p>
        <p class="author">{{ this.quote.author }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import Clock from "./clock/Clock";

import axios from "axios";

const keys = {
  backColor: "",
  showSec: "showSec",
  quote: "quote"
};

export default {
  name: "App",
  components: {
    Clock
  },
  data() {
    return {
      showSec: true,
      hour: 24,
      min: 24,
      sec: 24,
      quote: {
        text: "",
        author: "",
        date: null
      }
    };
  },
  mounted() {
    chrome.topSites.get(function(value) {
      console.log(value);
    });

    let fetch = false;
    let date = new Date();
    let dateString = `${date.getFullYear()}-${date.getMonth() +
      1}-${date.getDate()}`;
    let lastQuote = localStorage.getItem(keys.quote);
    if (lastQuote) {
      lastQuote = JSON.parse(lastQuote);
      let lastDate = lastQuote.date;
      if (lastDate !== dateString) {
        fetch = true;
      } else {
        this.quote = lastQuote;
      }
    } else {
      fetch = true;
    }

    if (fetch) {
      axios.get("http://quotes.rest/qod.json").then(res => {
        const { quotes } = res.data.contents;
        const quote = {
          text: quotes[0].quote,
          author: quotes[0].author,
          date: quotes[0].date
        };
        localStorage.setItem(keys.quote, JSON.stringify(quote));
        this.quote = quote;
      });
    }

    setInterval(() => {
      let date = new Date();
      this.hour = date.getHours();
      this.min = date.getMinutes();
      this.sec = date.getSeconds();
    }, 1000);
  }
};
</script>

<style>
body {
  background: white;
  margin: 0;
  width: 100%;
  text-align: center;
  font-family: Times, serif;
  color: rgb(75, 78, 78);
  background: #e2e2dd;
}
.clock {
  margin-top: 20vh;
}
.author {
  font-size: 1.2vw;
}
.quote {
  margin-top: 5vh;
  font-size: 1.7vw;
}
.quote-container {
  margin-left: 25vw;
  margin-right: 25vw;
  width: 50vw;
}
</style>

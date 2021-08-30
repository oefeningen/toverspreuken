<template>
  <q-page>
    <q-btn color="primary" icon="record" label="Listen" @click="Init" />
    <p>result : {{ result }}</p>
    <p>status : {{ status }}</p>
    <q-btn color="red" icon="check" label="muziek" @click="playMusic" />
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import { Howl, Howler } from "howler";

const SpeechRecognition =
  window.SpeechRecognition || window.webkitSpeechRecognition;

export default defineComponent({
  name: "PageIndex",
  data() {
    return {
      result: null,
      status: null,
    };
  },
  methods: {
    playMusic() {
      var sound = new Howl({ src: "intromusic.mp3" });
      sound.play();
    },
    Init() {
      speechSynthesis.speak(
        new SpeechSynthesisUtterance("Say the magic words")
      );
      console.log("init");
      const recognition = new SpeechRecognition();
      recognition.lang = "nl-BE";
      recognition.onerror = function (event) {
        console.log(event.error);
      };
      recognition.continuous = true;

      recognition.interimResults = true;
      recognition.start();
      console.log("init2");
      this.status = "Beginnen";
      recognition.onresult = (event) => {
        console.log("luisterresultaten...+");
        const last = event.results.length - 1;
        const res = event.results[last];
        const text = res[0].transcript;
        if (res.isFinal) {
          this.status = "processing ....";

          const response = this.process(text);

          this.result = `You said: ${text}`;
          this.status = "";
          // this.result.appendChild(p);

          // text to speech
          // speechSynthesis.speak(new SpeechSynthesisUtterance(response));
        } else {
          this.status = "Aan het luisteren...";
        }
      };
    },
    process(rawText) {
      let text = rawText.replace(/\s/g, "");
      text = text.toLowerCase();
      let response = null;
      switch (text) {
        case "wegaannaarschool":
          var sound = new Howl({ src: "intromusic.mp3" });
          sound.play();
          break;
        case "opeenshoorthijgeblaf":
          var sound = new Howl({ src: "hond.mp3" });
          sound.play();
          break;

        case "what'syourname":
          response = "My name's Siri.";
          break;
        case "howareyou":
          response = "I'm good.";
          break;
        case "whattimeisit":
          response = new Date().toLocaleTimeString();
          break;
        case "stop":
          response = "Bye!!";
          toggleBtn();
      }
      if (!response) {
        // window.open(
        //   `http://google.com/search?q=${rawText.replace("search", "")}`,
        //   "_blank"
        // );
        // return `I found some information for ${rawText}`;
      }
      return response;
    },
  },
  mounted() {},
});
</script>

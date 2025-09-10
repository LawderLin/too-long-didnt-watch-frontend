<template>
  <div class="container">
    <h1>YouTube Video → Text</h1>
    <input v-model="url" placeholder="輸入 YouTube 連結" />
    <button @click="transcribe">轉換</button>
    <div v-if="embed_url" style="margin-top: 20px;">
      <iframe width="560" height="315" v-bind:src="embed_url" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    </div>
    <p id="transcription" v-if="text">{{ text }}</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      url: "",
      embed_url: "",
      text: ""
    };
  },
  methods: {
    async transcribe() {
      try {
        const res = await axios.get("http://too-long-didnt-watch.com/transcribe", {
          params: { youtube_url: this.url }
        });
        this.text = res.data.text;
        // URL: https://youtu.be/loay2imHq5E?si=-uveHzANKVyS1zPP 
        // embed: https://www.youtube.com/embed/loay2imHq5E?si=-uveHzANKVyS1zPP
        this.embed_url = this.url.replace("youtu.be/", "www.youtube.com/embed/");
      } catch (err) {
        alert("轉錄失敗: " + err);
      }
    }
  }
};
</script>

<style>
.container {
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
}
input {
  width: 80%;
  padding: 8px;
  margin-top: 10px;
}
button {
  margin-left: 10px;
  padding: 8px 16px;
}
pre {
  text-align: left;
  white-space: pre-wrap;
  margin-top: 20px;
  padding: 10px;
  background: #f4f4f4;
}
</style>

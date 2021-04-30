<template>
<div class="tinymce-editor">
  <div id="waveform" style="height: 400px;    background: linear-gradient(135deg,#bb71f3 0%,#3d4d91 100%);
"></div>

</div>
</template>
<script>

  export default {
    components: {
    },
    data () {
      return {

      }
    },
    mounted () {
      let waveform = document.querySelector('#waveform')
      this.siriWave = new SiriWave({
        container: waveform,
        width: window.innerWidth,
        height: window.innerHeight * 0.3,
        cover: true,
        speed: 0.03,
        style: 'ios',
        amplitude: 1.3,
        curveDefinition:  [
          { attenuation: -2, lineWidth: 1, opacity: 0.4 },
          { attenuation: -6, lineWidth: 1, opacity: 0.2 },
          { attenuation: 4, lineWidth: 1, opacity: 0.6, color: "173, 57, 76" },
          { attenuation: 2, lineWidth: 1, opacity: 0.8, color: "15, 82, 169" },
          { attenuation: 1, lineWidth: 1.5, opacity: 1 ,color: "15, 82, 169"},
        ],
        frequency: 2
      });
      this.siriWave.start()
    },
    methods: {
      pause() {
        this.siriWave.stop()
      },
      paly() {
        this.siriWave.start()
      },
    },

  }
</script>

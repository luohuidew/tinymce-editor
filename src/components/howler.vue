<template>

<div class="tinymce-editor">
  <h1>howler.js 以下是声音组件与编辑器没关系 https://github.com/goldfire/howler.js</h1>
  <button @click="laser" disabled>
     播放一段声音
  </button>
  <button @click="speed">
     加速
  </button>
  <button @click="speed2">
    恢复原倍速
  </button>
  <button @click="fade">
    减缓暂停
  </button>
  <button @click="fade2">
    恢复播放
  </button>
  <button @click="step">
    跳到50s
  </button>
</div>
</template>
<script>
  import {Howl, Howler} from 'howler';

  export default {

    data () {
      return {
        myValue: this.value
      }
    },
    mounted () {
      // 初始化一个音频类
      var _this = this
     window.sound =  this.sound = new Howl({
        src: ['/audio.mp3'],
        autoplay: true,
        preload: true,
        // loop: true,
        volume: 0.5,
        // sprite: { // 这样使用 this.sound.play('blast');  使用sprite时autoplay必须为falsh
        //   blast: [0, 5000],
        //   laser: [4000, 1000],
        //   winner: [6000, 5000]
        // },
        onend: function() {
          console.log('Finished!');
        },
        ontimeupdate() {
          console.log('updateTime')
        },
        onfade(id) {
          console.log(this.isPause)
          _this.isPause ? _this.sound.play(id) : _this.sound.pause(id)
        },
        onpause() {
          console.log('暂停')
          _this.isPause = true
        },
         onstop() {
          console.log('停止了')
        },
        onseek(e) {
          console.log('onseek', e)
          // 执行seek方法才会执行
        },
        onunlock() {
          console.log('onunlock')
        },
       onpos() {
         console.log('onpos')

       },

      });

      // 播放音频
      // var id1 = sound.play();
      this.id2 = this.sound.play();
      this.sound.rate(1.0, this.id2);
      // 改变全局音频声音大小
      Howler.volume(0.5);

      // 只想改变某个音频的大小可以在初始化的时候修改
      // const sound = new Howl({
      //   src: ['sound.webm', 'sound.mp3'],
      //   volume:0.5
      // });
    },
    methods: {
      laser(){
        this.sound.play('blast');
      },
      step() {
        var seek = this.sound.seek() || 0; //
        this.sound.seek(50);
        console.log(this.sound.duration(), seek )

      },

      speed(){
        this.sound.rate(1.5, this.id2);
      },
      speed2(){
        this.sound.rate(1.0, this.id2);
      },
      fade() {
        this.sound.fade(0.5, 0, 1000, this.id2);

      },
      fade2() {
        this.sound.fade(0, 0.5, 1000, this.id2);

      },
    },
  }
</script>

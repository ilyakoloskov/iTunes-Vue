<template>
  <div class="header__track bg-secondary header-inner">
    <div v-if="playing.id" class="header__track-track">
      <div
        id="track-cover"
        class="header__track-img"
        :style="{
          backgroundImage: 'url(' + playing.albumCover + ')',
        }"
      ></div>
      <div class="header__track-wrap">
        <div class="header__track-container">
          <div class="header__track-inner">
            <i-button
            @click="$emit('mixTracks')" 
            :class="{
              'header__track-btn-active':isMixTracks
            }"
            type="track-mix"
            id="track-mix" class="header__track-btn">
              <svg
                width="20"
                height="16"
                viewBox="0 0 22 18"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M0.338867 13.3564C0.338867 13.9805 0.804688 14.4199 1.47266 14.4199H3.54688C5.11133 14.4199 6.06055 13.9541 7.12402 12.6885L9.11914 10.3154L11.0791 12.6533C12.1689 13.9541 13.2412 14.4287 14.8145 14.4287H16.3789V16.3535C16.3789 16.8809 16.7129 17.2148 17.249 17.2148C17.4863 17.2148 17.7061 17.127 17.8818 16.9863L21.3447 14.1035C21.7754 13.752 21.7666 13.1807 21.3447 12.8291L17.8818 9.9375C17.7061 9.78809 17.4863 9.7002 17.249 9.7002C16.7129 9.7002 16.3789 10.0342 16.3789 10.5615V12.2842H14.8584C13.8652 12.2842 13.25 11.959 12.5293 11.1064L10.4902 8.68945L12.5381 6.25488C13.2676 5.38477 13.8301 5.08594 14.8057 5.08594H16.3789V6.83496C16.3789 7.3623 16.7129 7.69629 17.249 7.69629C17.4863 7.69629 17.7061 7.6084 17.8818 7.46777L21.3447 4.58496C21.7754 4.2334 21.7666 3.66211 21.3447 3.31055L17.8818 0.418945C17.7061 0.269531 17.4863 0.181641 17.249 0.181641C16.7129 0.181641 16.3789 0.515625 16.3789 1.04297V2.94141H14.8232C13.1973 2.94141 12.1689 3.39844 11.0176 4.78711L9.11914 7.0459L7.12402 4.68164C6.06055 3.41602 5.0498 2.94141 3.48535 2.94141H1.47266C0.804688 2.94141 0.338867 3.38965 0.338867 4.01367C0.338867 4.6377 0.813477 5.08594 1.47266 5.08594H3.40625C4.35547 5.08594 4.98828 5.40234 5.70898 6.26367L7.73926 8.68066L5.70898 11.1064C4.97949 11.9678 4.4082 12.2842 3.46777 12.2842H1.47266C0.813477 12.2842 0.338867 12.7324 0.338867 13.3564Z"
                  fill="#1C1C1E"
                />
              </svg>
            </i-button>
            <h3
              id="track-artist"
              class="header__track-artist color-text display-3"
            >
              {{ playing.artistName }}
            </h3>
            <!-- $emit('loop') -->
            <i-button
                :type="countLoop === 1 ? 'repeat' : countLoop === 2 ? 'repeat-once' : 'repeat'"  
                @click="loop(++countLoop)"
                id="track-repeat" 
                class="header__track-btn"
                :class="{
                  'header__track-btn-active': isLoopTrack || isLoopAlbum,
                }"
              >

            </i-button>
          </div>
          <div class="header__track-inner display-4">
              <i-current-time 
                :audio="audio"
              />
            <h3
              id="track-name"
              class="header__track-name color-alternate display-5"
            >
              {{ playing.trackName }}
            </h3>
              <i-duration-time
                :audio="audio"
              />
          </div>
        </div>
        <div id="progress-bar" class="header__progress">
          <i-input
            @input="$emit('updateProgress', $event.target.value)"
            :value="Math.floor(audio.currentTime)"
            min="0"
            :max="Math.floor(audio.duration)"
            step="1"
            id="track-duration"
            class="header__progress-bar"
            type="range"
            :style="{
              '--range-value-track-progress': audioProgress + '%',
            }"
          />
        </div>
      </div>
    </div>
    <div v-else class="header__track-slot">
      <svg
        width="30px"
        viewBox="0 0 256 315"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        preserveAspectRatio="xMidYMid"
      >
        <g>
          <path
            d="M213.803394,167.030943 C214.2452,214.609646 255.542482,230.442639 256,230.644727 C255.650812,231.761357 249.401383,253.208293 234.24263,275.361446 C221.138555,294.513969 207.538253,313.596333 186.113759,313.991545 C165.062051,314.379442 158.292752,301.507828 134.22469,301.507828 C110.163898,301.507828 102.642899,313.596301 82.7151126,314.379442 C62.0350407,315.16201 46.2873831,293.668525 33.0744079,274.586162 C6.07529317,235.552544 -14.5576169,164.286328 13.147166,116.18047 C26.9103111,92.2909053 51.5060917,77.1630356 78.2026125,76.7751096 C98.5099145,76.3877456 117.677594,90.4371851 130.091705,90.4371851 C142.497945,90.4371851 165.790755,73.5415029 190.277627,76.0228474 C200.528668,76.4495055 229.303509,80.1636878 247.780625,107.209389 C246.291825,108.132333 213.44635,127.253405 213.803394,167.030988 M174.239142,50.1987033 C185.218331,36.9088319 192.607958,18.4081019 190.591988,0 C174.766312,0.636050225 155.629514,10.5457909 144.278109,23.8283506 C134.10507,35.5906758 125.195775,54.4170275 127.599657,72.4607932 C145.239231,73.8255433 163.259413,63.4970262 174.239142,50.1987249"
            fill="#979797"
          ></path>
        </g>
      </svg>
    </div>

  </div>

</template>

<script>
import IDurationTime from '../UI/IDurationTime.vue';

export default {
  components: { IDurationTime },
  name: "i-header-track",
  data() {
    return {
      audioProgress: "",
      test: {},
      countLoop: 0
    };
  },
  props: {
    selected: {
      type: Object,
    },
    playing: {
      type: Object,
    },
    isPlaying: {
      type: Boolean,
    },
    isVolume: {
      type: Number,
    },
    audio: {
      type: Object,
    },
    isLoopTrack:{
      type: Boolean
    },
    isLoopAlbum:{
      type: Boolean
    },
    isMixTracks:{
      type: Boolean
    }
  },
  emits: ["updateVolume", "updateProgress", "loop", "mixTracks"],
  methods: {
    updateAudioProgress() {
      setInterval(() => {
        this.audioProgress =
          (this.audio.currentTime / this.audio.duration) * 100;
        // this.test = this.time;
      }, 25);
    },
    loop(countLoop){
      
      if(countLoop == 3){
        this.resetLoop()
        this.$emit('loop', countLoop)
        console.log('СБРОС')
      }else{
        this.$emit('loop', countLoop)
      }
    },
    resetLoop(){
      this.countLoop = 0
    }
  },
  computed: {

  },
  mounted() {
    this.updateAudioProgress();
  },
};
</script>

<style lang="sass">

</style>

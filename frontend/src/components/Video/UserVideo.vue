<template>
<div
	v-if="streamManager"
	id="videoBox"
>
	<OvVideo :stream-manager="streamManager"/>
	<article>
		<p class="m-0">{{ clientData }}</p>
		<p class="d-flex m-0">
			<button 
				:class="{active: isActive}"
				@click="videoToggle"
			>
				{{ myVideoStatus }}
			</button>

			<button 
				:class="{active: isActive}"
				@click="audioToggle"
			>
				{{ myAudioStatus }}
			</button>
		</p>
	</article>
</div>
</template>

<script>
import OvVideo from './OvVideo';
import { mapState } from 'vuex'
export default {
	name: 'UserVideo',

	components: {
		OvVideo,
	},

	props: {
		streamManager: Object,
	},

	computed: {
		...mapState([
			'credentials',
		]),
		clientData () {
			const { clientData } = this.getConnectionData();
			return clientData;
		},
		myVideoStatus(){
      let video = this.streamManager.stream.videoActive
      if(video){
        return 'π₯'
      }return 'π₯β'
    },
    myAudioStatus(){
      let audio = this.streamManager.stream.audioActive
      if(audio){
        return 'π'
      }return 'πβ'
    },
		isActive(){
			if(this.clientData === this.credentials.userName){
				return true
			}return false
		}
	},

	methods: {
		getConnectionData () {
			const { connection } = this.streamManager.stream;
			return JSON.parse(connection.data);
		},
		
    // μ¬μ©μ λΉλμ€ on/off
    videoToggle(){
			const video = this.streamManager.stream.videoActive
      if(video){
        this.streamManager.publishVideo(false)
        console.log('video ' + video)
      }else{
        this.streamManager.publishVideo(true)
        console.log('video ' + video)
        }
    },

    // μ¬μ©μ μ€λμ€ on/off
    audioToggle(){
      console.log(this.subscribers)
      const audio = this.streamManager.stream.audioActive
      if(audio){
        this.streamManager.publishAudio(false)
        console.log('audio ' + audio)
      }else{
        this.streamManager.publishAudio(true)
        console.log('audio ' + audio)
      }
    },
	},
};
</script>
<style scoped>
#videoBox{
	display: flex;
	flex-direction: column;
	align-items: center;
	font-weight: bolder;
}

article{
	display: flex;
	flex-direction: column;
	align-items: center;
}
article p button{
	margin: 0 .5vw 0 .5vw;
}

.active{
	cursor: pointer;
}
</style>

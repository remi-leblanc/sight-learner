<template>
	<div id="piano">
		<Note note="B" :black="false" class="white"/>
		<Note note="A" :black="true" class="white"/>
		<Note note="G" :black="true" class="white"/>
		<Note note="F" :black="true" class="white"/>
		<Note note="E" :black="false" class="white"/>
		<Note note="D" :black="true" class="white"/>
		<Note note="C" :black="true" class="white"/>
	</div>
</template>

<script>
import Note from '@/components/Note.vue'
import '@/assets/css/piano.css'
import * as Tone from 'tone'
export default {
	name: 'Piano',
	components: {
		Note,
	},
	data: function(){
		return {
			piano: null,
			keyboardAsNote: {
				'KeyQ': 'C',
				'Digit2': 'C#',
				'KeyW': 'D',
				'Digit3': 'D#',
				'KeyE': 'E',
				'KeyR': 'F',
				'Digit5': 'F#',
				'KeyT': 'G',
				'Digit6': 'G#',
				'KeyY': 'A',
				'Digit7': 'A#',
				'KeyU': 'B',
			}
		}
	},
	props: {
		
	},
	mounted: function() {
		this.piano = new Tone.Sampler({
			urls: {
			"C4": "C4.mp3",
			"D#4": "Ds4.mp3",
			"F#4": "Fs4.mp3",
			"A4": "A4.mp3",
			},
			release: 1,
			baseUrl: "https://tonejs.github.io/audio/salamander/",
		}).toDestination();

		window.addEventListener('keypress', (e) => {
			if(this.keyboardAsNote[e.code] !== undefined){
				this.$root.$emit('playPianoNote', this.keyboardAsNote[e.code]);
			}
		});

		this.$root.$on('playPianoNote', (note) => {
			this.piano.triggerAttackRelease(note + 4, '8n');
		});
	}
}
</script>

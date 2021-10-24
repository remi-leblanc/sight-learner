<template>
	<div class="note" :class="{'correct': isCorrect, 'wrong': isWrong}" v-on:click.stop="play">
		<Note :note="toDiese" class="black" v-if="black"/>
	</div>
</template>

<script>
import Note from '@/components/Note.vue'
export default {
	name: 'Note',
	component: {
		Note,
	},
	data: function(){
		return {
			isCorrect: false,
			isWrong: false,
		}
	},
	props: ['note', 'black'],
	computed: {
		toDiese: function() {
			return this.note + '#'
		}
	},
	methods: {
		play: function(){
			this.$root.$emit('playPianoNote', this.note);
		}
	},
	mounted: function(){
		this.$root.$on('correct', (note) => {
			if(note == this.note){
				this.isCorrect = true;
				this.$nextTick(() => {
					setTimeout(() => {
						this.isCorrect = false;
					}, 10);
				});
			}
		});

		this.$root.$on('wrong', (note) => {
			if(note == this.note){
				this.isWrong = true;
				this.$nextTick(() => {
					setTimeout(() => {
						this.isWrong = false;
					}, 10);
				});
			}
		});
	}
}
</script>

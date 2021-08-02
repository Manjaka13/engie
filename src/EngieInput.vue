<template>
	<div :class="current_class">
		<h3 class="engie-input__question">{{ question }}</h3>
		<div class="engie-input__separator"></div>
		<div class="engie-input__box">
			<form class="engie-input_form" @submit.prevent="answer">
				<input
					v-model="result"
					class="engie-input__input"
					type="text"
					:placeholder="question"
				/>
				<button class="engie-input__submit" type="submit">
					Continuer
				</button>
			</form>
			<p :class="error_caption_class">{{ error_caption }}</p>
		</div>
	</div>
</template>

<script>
import "./EngieInput.css";

export default {
	name: "EngieInput",
	props: {
		displayed: {
			type: Boolean,
			default: false,
		},
		question: {
			type: String,
			default: "Question ?",
		},
	},
	data() {
		return {
			result: "",
			current_class: "engie-input",
			error_caption: "",
			error_caption_class: "engie-input__caption",
		};
	},
	methods: {
		answer() {
			if (typeof this.result === "string" && this.result.length > 2) {
				this.error_caption = "";
				this.$emit("answer", this.result);
			} else this.error_caption = "Veuillez remplir convenablement.";
		},
		update_ui() {
			if (this.displayed) this.current_class = "engie-input";
			else this.current_class = "engie-input engie-input--hidden";
		},
	},
	watch: {
		displayed: {
			immediate: true,
			handler() {
				this.update_ui();
			},
		},
		error_caption: {
			immediate: true,
			handler() {
				if (this.error_caption.length <= 0)
					this.error_caption_class = "engie-input__caption";
				else this.error_caption_class = "engie-input__caption--active";
			},
		},
	},
};
</script>

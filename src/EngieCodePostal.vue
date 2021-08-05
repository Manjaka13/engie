<template>
	<div :class="displayed ? 'engie-code-postal' : 'engie-code-postal--hidden'">
		<h3 v-if="title.length > 0" class="engie-code-postal__title">
			<span class="engie-code-postal__back" @click="get_back">
				<FontAwesomeIcon class="icon" icon="caret-left" />
				Retour
			</span>
			{{ title }}
		</h3>
		<form class="engie-code-postal__form" @submit.prevent="submit">
			<div class="engie-code-postal__icon">
				<FontAwesomeIcon icon="envelope" />
			</div>
			<input
				v-model="input"
				class="engie-code-postal__input"
				type="text"
				placeholder="Votre code postal."
				@keyup="suggest"
			/>
			<button class="engie-code-postal__submit" type="submit">
				Continuer
			</button>
			<ul
				v-if="suggestion.length > 0"
				class="engie-code-postal__suggestion"
			>
				<li
					v-for="(code, key) in suggestion"
					:key="key"
					class="engie-code-postal__suggestion-item"
					@click="select_suggestion(key)"
				>
					{{ code }}
				</li>
			</ul>
		</form>
		<p class="engie-code-postal__error">{{ error }}</p>
	</div>
</template>

<script>
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { faEnvelope, faCaretLeft } from "@fortawesome/free-solid-svg-icons";
import "./EngieCodePostal.css";

library.add(faEnvelope);
library.add(faCaretLeft);

export default {
	name: "EngieCodePostal",
	components: {
		FontAwesomeIcon,
	},
	props: {
		title: {
			type: String,
			default: "",
		},
		suggestion_list: {
			type: Array,
			default: () => [],
		},
		displayed: {
			type: Boolean,
			default: false,
		},
	},
	data() {
		return {
			suggestion: [],
			input: "",
			error: "",
		};
	},
	methods: {
		get_back() {
			this.$emit("get_back");
		},
		suggest() {
			this.error = "";
			if (this.input.length >= 2) {
				const regex = new RegExp(`^${this.input}`, "gi");
				this.suggestion = this.suggestion_list.filter(str =>
					str.match(regex)
				);
			}
		},
		unsuggest() {
			this.suggestion = [];
		},
		select_suggestion(key) {
			this.input = this.suggestion[key];
			this.unsuggest();
		},
		submit() {
			this.error = "";
			this.unsuggest();
			if (this.input.length >= 2) {
				const result = this.suggestion_list.filter(
					str => str.toLowerCase() === this.input.toLowerCase()
				);
				if (result.length > 0) {
					this.$emit("choose", this.input);
					this.input = "";
				} else this.error = "Ce code postal semble inexistant.";
			} else this.error = "Veuillez entrer un code postal valide.";
		},
	},
};
</script>

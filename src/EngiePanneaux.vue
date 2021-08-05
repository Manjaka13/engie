<template>
	<div :class="displayed ? 'engie-panneaux' : 'engie-panneaux--hidden'">
		<h3 v-if="title.length > 0" class="engie-panneaux__title">
			<span class="engie-panneaux__back" @click="get_back">
				<FontAwesomeIcon class="icon" icon="caret-left" />
				Retour
			</span>
			{{ title }}
		</h3>
		<form class="engie-panneaux__form" @submit.prevent="submit">
			<input
				class="engie-panneaux__input"
				type="number"
				placeholder="Nombre de kVA"
				min="0"
				v-model="value"
			/>
			<button class="engie-panneaux__submit" type="submit">
				Continuer
			</button>
		</form>
	</div>
</template>

<script>
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { faCaretLeft } from "@fortawesome/free-solid-svg-icons";
import "./EngiePanneaux.css";

library.add(faCaretLeft);

export default {
	name: "EngiePanneaux",
	props: {
		title: {
			type: String,
			default: "",
		},
		displayed: {
			type: Boolean,
			default: false,
		},
	},
	data() {
		return {
			value: null,
		};
	},
	components: {
		FontAwesomeIcon,
	},
	methods: {
		submit() {
			if (this.value >= 0) {
				this.$emit("choose", this.value);
				this.value = null;
			}
		},
	},
};
</script>

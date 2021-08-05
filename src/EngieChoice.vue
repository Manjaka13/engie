<template>
	<div
		v-if="choices.length > 0"
		:class="displayed ? 'engie-choice' : 'engie-choice--hidden'"
	>
		<h3 v-if="title.length > 0" class="engie-choice__title">
			<span class="engie-choice__back" @click="get_back">
				<FontAwesomeIcon class="icon" icon="caret-left" />
				Retour
			</span>
			{{ title }}
		</h3>
		<div class="engie-choice__list">
			<EngieChoiceItem
				v-for="(choice, key) in choices"
				:key="key"
				:icon="choices_icon.length > key ? choices_icon[key] : ''"
				:choice="choice"
				:label="choices_label.length > key ? choices_label[key] : ''"
				@choose="get_choice"
			/>
		</div>
		<slot name="slot" />
	</div>
</template>

<script>
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { faCaretLeft } from "@fortawesome/free-solid-svg-icons";
import "./EngieChoice.css";
import EngieChoiceItem from "./EngieChoiceItem.vue";

library.add(faCaretLeft);

export default {
	name: "EngieChoice",
	components: {
		FontAwesomeIcon,
		EngieChoiceItem,
	},
	props: {
		title: {
			type: String,
			default: "",
		},
		choices_icon: {
			type: Array,
			default: () => [],
		},
		choices: {
			type: Array,
			default: () => [],
		},
		choices_label: {
			type: Array,
			default: () => [],
		},
		displayed: {
			type: Boolean,
			default: false,
		},
	},
	methods: {
		get_choice(answer) {
			this.$emit("choose", answer);
		},
		get_back() {
			this.$emit("get_back");
		},
	},
};
</script>

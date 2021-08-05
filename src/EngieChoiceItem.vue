<template>
	<div class="engie-choice-item" @click.prevent="choose">
		<div class="engie-choice-item__icon">
			<FontAwesomeIcon :icon="icon.length > 0 ? icon : 'question'" />
		</div>
		<p v-if="choice.length > 0" class="engie-choice-item__choice">
			{{ choice }}
		</p>
		<p v-if="label.length > 0" class="engie-choice-item__label">
			{{ label }}
		</p>
	</div>
</template>

<script>
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { fas } from "@fortawesome/free-solid-svg-icons";
import "./EngieChoiceItem.css";

library.add(fas);

export default {
	name: "EngieChoiceItem",
	components: {
		FontAwesomeIcon,
	},
	props: {
		icon: {
			type: String,
			default: "question",
		},
		choice: {
			type: String,
			default: "???????",
		},
		label: {
			type: String,
			default: "",
		},
	},
	methods: {
		choose() {
			setTimeout(() => {
				this.$emit("choose", this.choice.toLowerCase());
			}, 200);
		},
	},
};
</script>

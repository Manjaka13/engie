<template>
	<div :class="current_class">
		<h3 class="engie-choice__question">{{ question }}</h3>
		<div class="engie-choice__separator"></div>
		<div class="engie-choice__box">
			<div
				class="engie-choice__cblock"
				v-for="(c, key) in choice"
				:key="key"
			>
				<div class="engie-choice__choice" @click.prevent="answer(c)">
					<p class="engie-choice__text">{{ c }}</p>
				</div>
				<p v-if="label.length > 0" class="engie-choice__label">
					{{ label[key] }}
				</p>
			</div>
		</div>
		<slot name="additionnal" />
	</div>
</template>

<script>
import "./EngieChoice.css";

export default {
	name: "EngieChoice",
	props: {
		displayed: {
			type: Boolean,
			default: false,
		},
		question: {
			type: String,
			default: "Question ?",
		},
		choice: {
			type: Array,
			default: () => [],
		},
		label: {
			type: Array,
			default: () => [],
		},
	},
	data() {
		return {
			current_class: "engie-choice",
		};
	},
	methods: {
		answer(result) {
			this.$emit("answer", result);
		},
		update_ui() {
			if (this.displayed) this.current_class = "engie-choice";
			else this.current_class = "engie-choice engie-choice--hidden";
		},
	},
	watch: {
		displayed: {
			immediate: true,
			handler() {
				this.update_ui();
			},
		},
	},
};
</script>

<template>
	<div class="engie-tree">
		<ul v-if="list" class="engie-tree__list">
			<li class="engie-tree__item" v-for="(item, key) in list" :key="key">
				<div class="engie-tree__step">{{ key + 1 }}</div>
				<p class="engie-tree__name">{{ item }}</p>
				<div v-if="key < list.length - 1" class="engie-tree__arrow">
					{{ ">" }}
				</div>
			</li>
		</ul>
		<ul v-if="!list" class="engie-tree__list">
			<li class="engie-tree__item">
				<p class="engie-tree__name">Redirection...</p>
			</li>
		</ul>
		<button v-if="list" class="engie-tree__back" @click="back">
			Retour
		</button>
	</div>
</template>

<script>
import "./EngieTree.css";

export default {
	name: "EngieTree",
	props: {
		list: {
			type: Array,
			default: () => [],
		},
	},
	methods: {
		back() {
			this.$emit("back");
		},
	},
};
</script>

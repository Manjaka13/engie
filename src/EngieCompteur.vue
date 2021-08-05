<template>
	<div :class="displayed ? 'engie-compteur' : 'engie-compteur--hidden'">
		<h3 v-if="title.length > 0" class="engie-compteur__title">
			<span class="engie-compteur__back" @click="get_back">
				<FontAwesomeIcon class="icon" icon="caret-left" />
				Retour
			</span>
			{{ title }}
		</h3>
		<form class="engie-compteur__form" @submit.prevent="submit">
			<div class="engie-compteur__box">
				<div v-if="display_electricite" class="engie-compteur__type">
					<h4 class="title">Electricité</h4>
					<div
						v-if="display_electricite"
						class="engie-compteur__group"
					>
						<div class="icon">
							<FontAwesomeIcon icon="bolt" />
						</div>
						<input
							class="input"
							type="number"
							min="0"
							placeholder="Normal"
							v-model="electricite"
							required
						/>
						<p class="unit">(kWh)</p>
					</div>
					<div v-if="display_creuse" class="engie-compteur__group">
						<div class="icon">
							<FontAwesomeIcon icon="car-battery" />
						</div>
						<input
							class="input"
							type="number"
							min="0"
							placeholder="Creuse"
							v-model="creuse"
							required
						/>
						<p class="unit">(kWh)</p>
					</div>
					<div
						v-if="display_exclusif_nuit"
						class="engie-compteur__group"
					>
						<div class="icon">
							<FontAwesomeIcon icon="moon" />
						</div>
						<input
							class="input"
							type="number"
							min="0"
							placeholder="Exckusif nuit"
							v-model="exclusif_nuit"
							required
						/>
						<p class="unit">(kWh)</p>
					</div>
				</div>
				<div v-if="display_gaz" class="engie-compteur__type">
					<h4 class="title">Gaz</h4>
					<div v-if="display_gaz" class="engie-compteur__group">
						<div class="icon">
							<FontAwesomeIcon icon="burn" />
						</div>
						<input
							class="input"
							type="number"
							min="0"
							placeholder="Normal"
							v-model="gaz"
							required
						/>
						<p class="unit">(kWh)</p>
					</div>
				</div>
			</div>
			<button class="submit" type="submit">Continuer</button>
		</form>
	</div>
</template>

<script>
import "./EngieCompteur.css";
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import {
	faCaretLeft,
	faBolt,
	faCarBattery,
	faMoon,
	faBurn,
} from "@fortawesome/free-solid-svg-icons";

library.add(faBolt);
library.add(faCaretLeft);
library.add(faCarBattery);
library.add(faMoon);
library.add(faBurn);

export default {
	name: "EngieCompteur",
	components: {
		FontAwesomeIcon,
	},
	props: {
		title: {
			type: String,
			default: "",
		},
		display_electricite: {
			type: Boolean,
			default: false,
		},
		display_creuse: {
			type: Boolean,
			default: false,
		},
		display_exclusif_nuit: {
			type: Boolean,
			default: false,
		},
		display_gaz: {
			type: Boolean,
			default: false,
		},
		displayed: {
			type: Boolean,
			default: false,
		},
	},
	data() {
		return {
			electricite: null,
			creuse: null,
			exclusif_nuit: null,
			gaz: null,
		};
	},
	methods: {
		submit() {
			this.$emit("choose", {
				electricite: this.electricite,
				creuse: this.creuse,
				exclusif_nuit: this.exclusif_nuit,
				gaz: this.gaz,
			});
		},
		get_back() {
			this.$emit("get_back");
		},
	},
};
</script>

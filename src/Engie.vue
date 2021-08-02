<template>
	<div class="engie">
		<EngieTree :list="tree" @back="previous" />
		<div v-if="step >= 0" class="engie__wrapper">
			<EngieChoice
				:displayed="current === 'deja_client'"
				question="Êtes-vous déjà client(e) chez nous ?"
				:choice="['OUI', 'NON']"
				@answer="get_answer"
			/>
			<EngieChoice
				:displayed="current === 'fournisseur_energie'"
				question="Avez-vous actuellement un autre fournisseur d’énergie ?"
				:choice="['OUI', 'NON']"
				@answer="get_answer"
			/>
			<EngieInput
				:displayed="current === 'code_postal'"
				question="Quel est votre code postal ?"
				@answer="get_answer"
			/>
			<EngieChoice
				:displayed="current === 'choix_calcul'"
				question="Quel prix voulez-vous calculer ?"
				:choice="['Electricité', 'Gaz', 'Electricité & Gaz']"
				@answer="get_answer"
			/>
			<EngieChoice
				:displayed="current === 'connais_consommation'"
				question="Connaissez-vous votre consommation d'énergie par an ?"
				:choice="['OUI', 'NON']"
				:label="[
					'Sur votre facture annuelle, vous voyez combien vous avez consommé.',
					'Nous allons vous poser quelques questions afin d’évaluer votre consommation d’énergie.',
				]"
				@answer="get_answer"
			/>
			<EngieChoice
				:displayed="current === 'type_compteur'"
				question="De quel type de compteur d’électricité disposez-vous ?"
				:choice="['Normal', 'Bihoraire']"
				:label="[
					'Un tarif normal.',
					'Heures pleines et heures creuses.',
				]"
				@answer="get_answer"
			>
				<template #additionnal>
					<form class="engie__compteur">
						<input
							class="engie__checkbox"
							type="checkbox"
							id="additionnal"
							name="subscribe"
							value="newsletter"
						/>
						<label class="engie__label" for="additionnal"
							>J'ai également un compteur exclusif nuit.</label
						>
					</form>
				</template>
			</EngieChoice>
		</div>
		<div v-else class="engie-redirect">
			<div class="loader">Loading...</div>
		</div>
	</div>
</template>

<script>
import "./Engie.css";
import EngieTree from "./EngieTree.vue";
import EngieChoice from "./EngieChoice.vue";
import EngieInput from "./EngieInput.vue";

export default {
	name: "Engie",
	components: {
		EngieTree,
		EngieChoice,
		EngieInput,
	},
	data() {
		return {
			current: "deja_client",
			step: -1,
			step_name: ["Simulation", "Offre", "Commande", "Confirmation"],
			tree: [],
			answer: [],
			last_current: null,
		};
	},
	mounted() {
		this.step_up();
	},
	methods: {
		step_up() {
			this.step++;
			this.tree.push(this.step_name[this.step]);
		},
		step_down() {
			if (this.step > 0) {
				this.step--;
				this.tree.pop();
				this.answer.pop();
			}
		},
		step_stop() {
			this.step = -1;
			this.tree = null;
		},
		next(name) {
			if (!name) this.step_stop();
			else {
				if (this.current) this.last_current = this.current;
				this.current = name;
			}
		},
		previous() {
			if (this.last_current) this.next(this.last_current);
		},
		get_answer(answer) {
			let tmp = [...this.answer];
			tmp.push(answer);
			this.answer = [...tmp];
			console.log(answer);
			switch (this.current) {
				case "deja_client":
					console.log(this.current, answer);
					if (answer === "OUI") {
						this.next();
					} else {
						this.next("fournisseur_energie");
					}
					break;
				case "fournisseur_energie":
					this.next("code_postal");
					break;
				case "code_postal":
					this.next("choix_calcul");
					break;
				case "choix_calcul":
					this.next("connais_consommation");
					break;
				case "connais_consommation":
					if (answer === "OUI") this.next("type_compteur");
					break;
				case 4:
					if (answer === "NON") {
						this.index += 2;
						return;
					}
					break;
				default:
					break;
			}
		},
	},
	watch: {
		answer: {
			immediate: true,
			handler() {
				console.log(this.answer);
			},
		},
	},
};
</script>

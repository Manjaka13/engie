<template>
	<div :class="current_page === 'resultat' ? 'engie-resultat' : 'engie'">
		<EngieChoice
			:displayed="current_page === 'fournisseurEnergie'"
			title="Avez-vous actuellement un autre fournisseur d’énergie ?"
			:choices_icon="['times', 'check']"
			:choices="['Oui', 'Non']"
			:choices_label="[]"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'prixCalcul'"
			title="Quel prix voulez-vous calculer ?"
			:choices_icon="['plug', 'burn', 'charging-station']"
			:choices="['Electricité', 'Gaz', 'Electricité et Gaz']"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'consommationAnnuel'"
			title="Connaissez-vous votre consommation d'énergie par an ?"
			:choices_icon="['times', 'check']"
			:choices="['Oui', 'Non']"
			:choices_label="[
				'Sur votre facture annuelle, vous voyez combien vous avez consommé.',
				'Nous allons vous poser quelques questions afin d’évaluer votre consommation d’énergie.',
			]"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'proprietaireLocataire'"
			title="Etes-vous propriétaire ou locataire ?"
			:choices_icon="['house-user', 'male']"
			:choices="['Propriétaire', 'Locataire']"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'typeLogement'"
			title="De quel type de logement disposez-vous ?"
			:choices_icon="['home', 'city', 'hotel', 'building']"
			:choices="[
				'Appartement',
				'Maison mitoyenne',
				'Maison 3 façades',
				'Maison 4 façades',
			]"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'chauffageLogement'"
			title="Comment chauffez-vous votre logement ?"
			:choices_icon="[
				'plug',
				'burn',
				'oil-can',
				'pump-soap',
				'thermometer-quarter',
			]"
			:choices="[
				'Electricité',
				'Gaz',
				'Mazout',
				'Pompe à chaleur',
				'Autre manières',
			]"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'panneauxSolaires'"
			title="Avez-vous des panneaux solaires?"
			:choices_icon="['times', 'check']"
			:choices="['Oui', 'Non']"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'nombrePersonnes'"
			title="De combien de personnes se compose votre ménage ?"
			:choices_icon="[
				'user',
				'user-friends',
				'users',
				'user-plus',
				'plus',
			]"
			:choices="[
				'1 personne',
				'2 personnes',
				'3 personnes',
				'4 personnes',
				'5 personnes ou plus',
			]"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'dateConstruction'"
			title="De quand date la construction de votre logement ?"
			:choices_icon="['calendar-plus', 'calendar-minus']"
			:choices="['De 2000 ou après', 'Avant 2000']"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieChoice
			:displayed="current_page === 'isolationHabitation'"
			title="Votre habitation a-t-elle été isolée ?"
			:choices_icon="['times', 'check']"
			:choices="['Oui', 'Non']"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngieCodePostal
			:displayed="current_page === 'codePostal'"
			title="Quel est votre code postal ?"
			:suggestion_list="list_codePostal"
			@choose="get_answer"
			@get_back="get_back"
		/>
		<EngiePanneaux
			:displayed="current_page === 'puissancePanneaux'"
			title="Quelle est la puissance de votre installation ?"
			@choose="get_answer"
		/>
		<EngieChoice
			:displayed="current_page === 'typeCompteur'"
			title="De quel type de compteur d’électricité disposez-vous ?"
			:choices_icon="['calculator', 'moon']"
			:choices="['Normal', 'Bihoraire']"
			:choices_label="[
				'Tarif normal',
				'Heures pleines et heures creuses',
			]"
			@choose="get_answer"
			@get_back="get_back"
		>
			<template #slot>
				<form class="engie__tarif-nuit">
					<input
						class="engie__checkbox"
						type="checkbox"
						id="tarif_nuit"
						name="subscribe"
						v-model="tarif_nuit"
					/>
					<label
						class="engie__label"
						for="tarif_nuit"
						title="Un compteur exclusif nuit est un compteur séparé qui alimente certains appareils électriques en électricité durant la nuit (comme des appareils de chauffage par accumulation ou des boilers électriques). Un compteur exclusif nuit est toujours combiné à un compteur normal ou un compteur bihoraire."
					>
						J'ai également un compteur exclusif nuit
					</label>
				</form>
			</template>
		</EngieChoice>
		<EngieCompteur
			:displayed="current_page === 'energieAnnuel'"
			title="Combien d'énergie consommez-vous par an ?"
			:display_electricite="
				answer.prixCalcul === 'electricité et gaz' ||
					answer.prixCalcul === 'electricité'
			"
			:display_creuse="answer.typeCompteur === 'bihoraire'"
			:display_exclusif_nuit="tarif_nuit ? true : false"
			:display_gaz="
				answer.prixCalcul === 'electricité et gaz' ||
					answer.prixCalcul === 'gaz'
			"
			@get_back="get_back"
			@choose="get_answer"
		/>

		<div v-if="current_page === 'resultat'" class="engie__resultat">
			<h2 class="engie__title">Nos suggestions</h2>
			<div class="engie__suggestion">
				<div class="engie__resultat-item">
					<h3 class="title">Direct</h3>
					<p class="erased_price"></p>
					<div class="separator"></div>
					<p class="price">{{ prix1 }} €</p>
					<p class="month">par mois</p>
					<!-- <p class="text">Le meilleur prix variable de l'énergie.</p>
					<div class="box">
						<FontAwesomeIcon class="icon" icon="check-circle" />
						<p class="label">Prix variable</p>
					</div>
					<div class="box">
						<FontAwesomeIcon class="icon" icon="check-circle" />
						<p class="label">Votre facture par e-mail</p>
					</div> -->
				</div>
				<div class="engie__resultat-item engie__resultat-item--special">
					<h3 class="title">Easy</h3>
					<!-- <p class="erased_price">10,87 €</p> -->
					<div class="separator"></div>
					<p class="price">{{ prix2 }} €</p>
					<p class="month">par mois</p>
					<!-- <p class="text">
						Un prix de l'énergie fixe jusqu'à août 2024
					</p>
					<div class="box">
						<FontAwesomeIcon class="icon" icon="check-circle" />
						<p class="label">Prix fixe</p>
					</div>
					<div class="box">
						<FontAwesomeIcon class="icon" icon="check-circle" />
						<p class="label">
							Votre facture via e-mail ou par courrier
						</p>
					</div>
					<p class="banner">Prix fixe le plus bas</p> -->
				</div>
				<div class="engie__resultat-item">
					<h3 class="title">Easy3</h3>
					<p class="erased_price"></p>
					<div class="separator"></div>
					<p class="price">{{ prix3 }} €</p>
					<p class="month">par mois</p>
					<!-- <p class="text">
						Un prix de l'énergie fixe jusqu'à août 2024
					</p>
					<div class="box">
						<FontAwesomeIcon class="icon" icon="check-circle" />
						<p class="label">Prix fixe</p>
					</div>
					<div class="box">
						<FontAwesomeIcon class="icon" icon="check-circle" />
						<p class="label">
							Votre facture via e-mail ou par courrier
						</p>
					</div>
					<p class="promo">
						<FontAwesomeIcon class="star" icon="star" /> Promo
					</p> -->
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import axios from "axios";
import "./Engie.css";
import EngieChoice from "./EngieChoice.vue";
import EngieCodePostal from "./EngieCodePostal.vue";
import EngiePanneaux from "./EngiePanneaux.vue";
import EngieCompteur from "./EngieCompteur.vue";
// import { library } from "@fortawesome/fontawesome-svg-core";
// import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
// import { faCheckCircle, faStar } from "@fortawesome/free-solid-svg-icons";

// library.add(faCheckCircle);
// library.add(faStar);

export default {
	name: "Engie",
	components: {
		EngieChoice,
		EngieCodePostal,
		EngiePanneaux,
		EngieCompteur,
		// FontAwesomeIcon,
	},
	data() {
		return {
			last_page: [],
			current_page: "fournisseurEnergie",
			tarif_nuit: null,
			answer: {},
			list_codePostal: [],
			prix1: 30.99,
			prix2: 90.99,
			prix3: 58.99,
		};
	},
	created() {
		axios
			.get("https://run.mocky.io/v3/e62df618-0382-4c31-92c3-cc4084719ee1")
			.then(({ data }) => {
				this.list_codePostal = [...data];
			})
			.catch(e => {
				console.error(e);
			});
	},
	methods: {
		advance(next, answer) {
			this.last_page.push(this.current_page);
			this.answer[this.current_page] = answer;
			this.current_page = next;
			console.log("Advance to ", next);
		},
		get_answer(answer) {
			switch (this.current_page) {
				case "fournisseurEnergie":
					this.advance("codePostal", answer);
					break;
				case "codePostal":
					this.advance("prixCalcul", answer);
					break;
				case "prixCalcul":
					this.advance("consommationAnnuel", answer);
					break;
				case "consommationAnnuel":
					if (answer === "oui") {
						if (this.answer.prixCalcul === "gaz")
							this.advance("energieAnnuel", answer);
						else this.advance("typeCompteur", answer);
					} else this.advance("nombrePersonnes", answer);
					break;
				case "nombrePersonnes":
					this.advance("dateConstruction", answer);
					break;
				case "energieAnnuel":
					this.advance("proprietaireLocataire", answer);
					break;
				case "dateConstruction":
					if (answer === "de 2000 ou après")
						this.advance("resultat", answer);
					else this.advance("isolationHabitation", answer);

					break;
				case "isolationHabitation":
					this.advance("resultat", answer);
					break;
				case "typeCompteur":
					this.advance("energieAnnuel", answer);
					break;
				case "proprietaireLocataire":
					if (answer === "locataire")
						this.advance("resultat", answer);
					else this.advance("typeLogement", answer);

					break;
				case "typeLogement":
					this.advance("chauffageLogement", answer);
					break;
				case "chauffageLogement":
					this.advance("resultat", answer);
					break;
				case "panneauxSolaires":
					if (answer === "oui")
						this.advance("puissancePanneaux", answer);
					else this.advance("resultat", answer);
					break;
				case "puissancePanneaux":
					this.advance("resultat", answer);
					break;
			}
			console.log(this.answer);
		},
		get_back() {
			if (this.last_page.length > 0) {
				this.current_page = this.last_page.pop();
				this.answer[this.current_page] = null;
				console.log("Back to ", this.current_page);
			}
			console.log(this.answer);
		},
	},
	watch: {
		tarif_nuit: {
			immediate: true,
			handler() {
				console.log(this.tarif_nuit);
			},
		},
	},
};
</script>

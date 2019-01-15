<template>
	<div class="ui container">
		<h1>Sondage du Jour</h1>
		<form>
			<p>{{ question }}</p>
			<div class="field" v-for="reponse in reponses">
				<div class="ui radio checkbox" >
					<input type="radio" name="answer" :id="reponse" checked>
					<label :for="reponse">{{ reponse }}</label>
				</div>
			</div>
			<div class="field">
				<button class="ui primary basic button" @click.prevent="vote">Valider</button>
			</div>
		</form>
		<div class="ui green message center aligned" v-show="visible"><i class="close icon" @click="close" title="Fermer"></i>Merci de votre participation !</div>
		<Results :datas="values" :total="total"/>
	</div>
</template>

<script>
import Results from './Results'
export default {
	name: 'Sondage',
	data () {
		return {
			visible: false,
			total: null,
			values: null,
			question: null,
			reponses: null
		}
	},
	components: {
		Results
	},
	mounted () {
		// On affiche le sondage en cours aprés que l'application soit montée
		this.$http.get('http://localhost/sondage/static/datas.json').then(response => {
			this.question = response.body[0].question // Question
			this.reponses = Object.keys(response.body[0].reponses) // Réponses
			this.values = response.body[0].reponses // Résultats des réponses
			this.total = response.body[0].total // Nombre total de réponses
		})
	},
	methods: {
		// Permet d'ajouter un vote au sondage
		vote () {
			let current;
			// Liste des éléments de type 'input'
			let inputs = document.querySelectorAll('input[type="radio"]')
			// Sur l'ensemble des éléments 
			for (let i = 0; i < inputs.length; i++) {
				// On vérifie l'élément coché
				if (inputs[i].checked) {
					// On stocke l'élément dans la propriété 'current'
					current = inputs[i].id
					this.values[current] += 1
					// On incrémente le nombre total de votes
					this.total += 1
				}
			}
			// Affiche un message de confirmation à l'utilisateur
			this.visible = true
		},
		// Masque le message de l'utilisateur
		close () {
			this.visible = false
		}
	}
}
</script>

<style>
	* {
		user-select: none;
	}
	a {
		cursor: pointer;
	}
	.container {
		border: 2px solid #000;
		padding: 20px;
		border-radius: 10px;
		margin-top: 30px;
		background-color: #FFF;
	}
	div.field {
		margin-bottom: 15px;
	}
</style>

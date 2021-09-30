<template>
	<v-app>
		<v-main class="dogs-layout">
			<v-container fill-height>
				<div class="dogs-overlay">
					<h1 class="display-2 text-xs-center">Choose your favorite dogs</h1>
					<v-card class="dog-card">
						<v-img height="400px" :src="currentDogLink"></v-img>
						<v-card-actions>
							<v-spacer></v-spacer>
							<v-btn icon @click="addToFavorites" v-if="!isAlreadyInFavorites"> <!-- :disabled could have also been used instead of the v-if -->
								<v-icon>favorite</v-icon>
							</v-btn>
							<v-btn icon @click="loadNewDog">
								<v-icon>forward</v-icon>
							</v-btn>
						</v-card-actions>
					</v-card>
					<v-container grid-list-md fluid>
						<v-layout wrap>
							<v-flex xs6 sm4 md2 v-for="pet in favoriteDogs" :key="pet">
								<v-card class="dog-card">
									<v-img height="150px" :src="pet"></v-img>
									<v-card-actions>
										<v-spacer></v-spacer>
										<v-btn icon @click="removeFromFavorites(pet)">
											<v-icon>delete</v-icon>
										</v-btn>
									</v-card-actions>
								</v-card>
							</v-flex>
						</v-layout>
				</v-container>
				</div>
			</v-container>
		</v-main>
	</v-app>
</template>


<script>
import axios from 'axios';
export default {
	data(){
		return{
			currentDogLink: "",
			favoriteDogs:[]
		};
	},
	methods:{
		//This function performs the API call using the AXIOS library/dependency
		loadNewDog(){
			axios
				.get('https://dog.ceo/api/breeds/image/random')
				.then((response) => {
					this.currentDogLink = response.data.message;
				})
				.catch((error) => {
					console.log(error);
				});

		},
		addToFavorites(){
			this.favoriteDogs.push(this.currentDogLink);
		}, 
		//Deletes the dog from the array by using the filter method 
		//The Filter function is saving every element in the array, but pet, which is de dog that needs to be deleted
		removeFromFavorites(pet){
			console.log(pet);
			this.favoriteDogs = this.favoriteDogs.filter(currentElement => {
				return currentElement!==pet;
			})
		}
	},
	//This hook takes place right after the component is created 
	created(){
		this.loadNewDog();
	},
	computed:{
		//Checks is the the current dog being displayed is already in the array favoriteDogs 
		//The indexOf() method returns the first index at which a given element can be found in the array, or -1 if it is not present. 
		isAlreadyInFavorites(){
			return this.favoriteDogs.indexOf(this.currentDogLink) > -1;
		}
	}

}
</script>

<style>
img {
  max-width: 100%;
}

h1 {
  padding-bottom: 15px;
}

.dogs-layout {
  width: 100%;
  background: #fff center repeat;
  background-image: url("https://github.com/FrontEndFoxes/projects/blob/main/petshop/images/bg3.jpg?raw=true");
}

.dogs-overlay {
  width: 100%;
  padding: 20px;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
}

@media (max-width: 768px) {
  .dogs-overlay {
    margin: 0;
  }
}

.dog-card {
  width: 100%;
  max-width: 600px;
}
</style>

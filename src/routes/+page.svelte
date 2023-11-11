<!-- 
				here be spaghetti

				todo:
				- autosave fields to localstorage
-->
<main class="container px-4 py-2 bg-gray-900 text-white min-h-full min-w-full">
<header class="text-center">
				<h1 class="text-2xl font-semibold m-4">Calorie Calculator</h1>
</header>
<div class="text-sm text-gray-400">
				<p>
								Calculate Your Daily Calorie Needs for Weight Loss, Gain, or Maintenance
				</p>
</div>

<div>
				<h2>Biological Gender</h2>
				<div class="flex justify-evenly">

								<label >
												<input
																type="radio"
																bind:group={gender}
																name="gender"
																value="male"
												/>
Male
								</label>


								<label>
												<input
																type="radio"
																bind:group={gender}
																name="gender"
																value="female"
												/>
Female
								</label>
				</div>
</div>

<div>
				<h2>Your Details</h2>
				<label class="pb-2"><p class="block pb-2">Height (cm)</p>
								<input 
												class="rounded border-gray-700 bg-gray-800 py-1 px-2 border w-full"
												type="number"
												bind:value={height}
							 />
				</label>

				<label class="pb-2"><p class="block pb-2">Weight (kg)</p>
								<input 
												class="rounded border-gray-700 bg-gray-800 py-1 px-2 border w-full"
												type="number"
											  bind:value={weight}
							 />
				</label>

				<label class="pb-2"><p class="block pb-2">Age (Years)</p>
								<input 
												class="rounded border-gray-700 bg-gray-800 py-1 px-2 border w-full"
												type="number"
											  bind:value={age}
							 />
				</label>
</div>

<div>
				<h2>Your Activity</h2>
				<p class="font-medium">How active are you?</p>

				<p class="text-sm text-gray-400">{exerciseDecoded}</p>
				<input 
								class="w-full"
								type="range" 
								min="0" 
								max="4" 
								step="0.01" 
								bind:value={exercise}
				/>


</div>

<div>
				<h2>Results</h2>
				<div class="flex justify-between">
								<div>
												<h3>Weight Loss</h3>
												<ul>
																{#each lossBreaks as b}
																				<li><strong>{getPerc(b)}:</strong> {~~(tdee * b)}</li>
																{/each}
												</ul>
								</div>

								<div>
												<h3>Weight Gain</h3>
												<ul>
																{#each gainBreaks as b}
																				<li><strong>{getPerc(b)}:</strong> {~~(tdee * b)}</li>
																{/each}
												</ul>
								</div>
				</div>


				<div class="flex justify-between pt-4">
								<p><strong>BMR: </strong><span>{bmr}</span></p>
								<p><strong>TDEE: </strong><span>{tdee}</span></p>
				</div>


</div>
</main>

<script lang="ts">
				const exerciseTDEEMod = [1.2, 1.375, 1.55, 1.725, 1.9]
				const getActivityVerb = [
								'Not active', 
								'Lightly active (1-3 days/week)',
								'Moderately active (3-5 days/week)',
								'Very active (5-6 days/week)',
								'Incredibly active (Multiple times a day, daily)'
				]

				let gender = 'male'
				let exercise = 3
				let exerciseDecoded = getActivityVerb[exercise]
				let height = 180
				let weight = 80
				let age = 28

				function getPerc(val: number) {
				return `${Math.round(val * 100)}%`
				}

				function calculateTDEE() {
								return Math.round(calculateBMR() * exerciseTDEEMod[Math.floor(exercise)])
				}

				function calculateBMR() {
								const bmrMod = {male: +5, female: -161}[gender] as number
								return Math.round((10 * weight) + (6.25 * height) - (4.92 * age) + bmrMod)
				}

				let tdee = calculateTDEE()
				let bmr = calculateBMR()

				let lossBreaks = [
								0.8,
								0.85,
								0.9,
								
				]

				let gainBreaks = [
								1.1,
								1.15,
								1.2,
				]

				$: {
								// why is this necessary
								gender = gender
								exercise = exercise
								height = height
								weight = weight
								age = age
								exerciseDecoded = getActivityVerb[Math.floor(exercise)]

								// uhhh
								tdee = calculateTDEE()
								bmr = calculateBMR()
				}
				
</script>

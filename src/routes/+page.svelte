<h1>Calorie Calculator</h1>
<div>
				<p>Visit <a href="https://kit.svelte.dev">https://hjf.io/calorie-calc/</a> to learn more</p>
</div>

<div>
				<h2>Biological Gender</h2>
				<div>

								<label>Male
												<input
																type="radio"
																bind:group={gender}
																name="gender"
																value="male"
												/>
								</label>


								<label>Female
												<input
																type="radio"
																bind:group={gender}
																name="gender"
																value="female"
												/>
								</label>
				</div>
</div>

<div>
				<h2>Your Details</h2>
				<label>Height (cm) 
								<input 
												type="number"
												bind:value={height}
							 />
				</label>
				<label>Weight (kg)
								<input 
												type="number"
											  bind:value={weight}
							 />
				</label>
				<label>Age (years)
								<input 
												type="number"
											  bind:value={age}
							 />
				</label>
</div>

<div>
				<h2>Your Activity</h2>

				<div>

								<label>No Exercise
												<input
																type="radio"
																bind:group={exercise}
																name="exercise"
																value="none"
												/>
								</label>


								<label>Light Exercise (1-3 days per week)
												<input
																type="radio"
																bind:group={exercise}
																name="exercise"
																value="light"
												/>
								</label>
				</div>
				<div>

								<label>Moderate Exercise (3-5 days per week)
												<input
																type="radio"
																bind:group={exercise}
																name="exercise"
																value="moderate"
												/>
								</label>


								<label>Very Active (6-7 days per week)
												<input
																type="radio"
																bind:group={exercise}
																name="exercise"
																value="very"
												/>
								</label>


								<label>Incredibly Active (Multiple times a day, every day)
												<input
																type="radio"
																bind:group={exercise}
																name="exercise"
																value="incredible"
												/>
								</label>
				</div>
</div>

<div>
				<h2>Results</h2>
				<p><strong>BMR: </strong><span>{bmr}</span></p>
				<p><strong>TDEE: </strong><span>{tdee}</span></p>
				<div>
								<ul>
												{#each breaks as b}
																<li><strong>{b}:</strong> {~~(tdee * b)}</li>
												{/each}
								</ul>
				</div>
</div>

<script lang="ts">
				let gender = 'male'
				let exercise = 'very'
				let height = 180
				let weight = 80
				let age = 28

				function calculateTDEE() {
								const tdeeMod = {
												none: 1.2,
												light: 1.375,
												moderate: 1.55,
												very: 1.725,
												incredible: 1.9,
								}[exercise] as number
								return calculateBMR() * tdeeMod
				}

				function calculateBMR() {
								const bmrMod = {male: +5, female: -161}[gender] as number
								return (10 * weight) + (6.25 * height) - (4.92 * age) + bmrMod
				}

				let tdee = calculateTDEE()
				let bmr = calculateBMR()

				let breaks = [
								0.8,
								0.85,
								0.9,
								1,
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

								// uhhh
								tdee = calculateTDEE()
								bmr = calculateBMR()
				}
				
</script>

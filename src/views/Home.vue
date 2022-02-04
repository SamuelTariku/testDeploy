<template>
	<div class="d-flex justify-content-center h-100">
		<div class="col-12 col-lg-6 text-center h-100 d-flex flex-column">
			<div
				class="d-flex flex-column justify-content-center mb-5"
				style="height: 300px"
			>
				<h1>Test Github Deploy</h1>
				<h2>Rock Paper Scissors</h2>
			</div>
			<div>
				<div class="">
					<!-- Rock Paper Scissors Options  -->
					<transition name="wipe-open" mode="out-in">
						<div
							class="flex-wrap justify-content-around"
							:class="{ 'd-flex': !resultShow }"
							v-if="!resultShow"
              key="1"
						>
							<div
								class="
									col-lg-3
									p-4
									mx-2
                  shadow-sm
									border
									rounded
									hoverEffect
								"
								@click="pickOption(0)"
							>
								<div
									class="
										mx-auto
										border
										d-flex
										align-items-center
										justify-content-center
										rounded
									"
									style="
										width: 80px;
										height: 80px;
										font-size: 30px;
										color: white;
										background: black;
									"
								>
									<i class="fas fa-hand-rock"></i>
								</div>
								<p class="my-2" style="font-weight: bold">
									Rock
								</p>
							</div>
							<div
								class="
									col-lg-3
									p-4
									mx-2
									shadow-sm
									border
									rounded
									hoverEffect
								"
								@click="pickOption(1)"
							>
								<div
									class="
										mx-auto
										border
										d-flex
										align-items-center
										justify-content-center
										rounded
									"
									style="
										width: 80px;
										height: 80px;
										font-size: 30px;
										color: white;
										background: black;
									"
								>
									<i class="fas fa-hand-paper"></i>
								</div>
								<p class="my-2" style="font-weight: bold">
									Paper
								</p>
							</div>

							<div
								class="
									col-lg-3
									p-4
									mx-2
									shadow-sm
									border
									rounded
									hoverEffect
								"
								@click="pickOption(2)"
							>
								<div
									class="
										mx-auto
										border
										d-flex
										align-items-center
										justify-content-center
										rounded
									"
									style="
										width: 80px;
										height: 80px;
										font-size: 30px;
										color: white;
										background: black;
									"
								>
									<i class="fas fa-hand-scissors"></i>
								</div>
								<p class="my-2" style="font-weight: bold">
									Scissors
								</p>
							</div>
						</div>
						<!-- Results -->
						<div v-else key="2">
							<div
								class="border rounded"
								:class="{
									'bg-success': this.win == 0,
									'bg-danger': this.win == 1,
									'bg-primary': this.win == 2,
								}"
							>
								<div class="p-5 rounded">
									<h3
										class="text-light"
										style="font-weight: bold"
										v-show="this.win == 0"
									>
										You Win
									</h3>
									<h3
										class=""
										style="font-weight: bold"
										v-show="this.win == 1"
									>
										You Lose
									</h3>
									<h3
										class=""
										style="font-weight: bold"
										v-show="this.win == 2"
									>
										Draw
									</h3>
								</div>
								<div class="py-4 bg-light">
									<div>
										The computer picked
										<span
											class="p-2"
											style="
												background: black;
												color: white;
											"
										>
											<i
												class="fas"
												:class="
													'fa-hand-' +
													(ai_choice == 0
														? 'rock'
														: ai_choice == 1
														? 'paper'
														: 'scissors')
												"
											></i>
										</span>
									</div>
								</div>
							</div>
							<button
								class="btn btn-primary my-4 btn-lg"
								@click="playAgain"
							>
								Play Again?
							</button>
						</div>
					</transition>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
// Play Options
class PlayOptions {
	static Rock = 0;
	static Paper = 1;
	static Scissors = 2;
}

// Win Options
class WinOptions {
	static Player1Win = 0;
	static Player2Win = 1;
	static Draw = 2;
}

export default {
	name: "Home",
	components: {},
	methods: {
		pickOption(optionIndex) {
			this.ai_choice = Math.floor(Math.random() * 3);
			this.win = this.rpsCompute(optionIndex, this.ai_choice);
			this.resultShow = true;
			console.log(
				this.win == WinOptions.Draw
					? "Draw"
					: this.win == WinOptions.Player1Win
					? "Player 1 wins"
					: "Player 2 wins"
			);
		},
		// Enter rps options returns who won
		rpsCompute(player1, player2) {
			if (player1 == player2) {
				return WinOptions.Draw;
			}

			switch (player1) {
				case PlayOptions.Rock:
					switch (player2) {
						case PlayOptions.Paper:
							return WinOptions.Player2Win;

						case PlayOptions.Scissors:
							return WinOptions.Player1Win;
					}
					break;
				case PlayOptions.Paper:
					switch (player2) {
						case PlayOptions.Scissors:
							return WinOptions.Player2Win;
						case PlayOptions.Rock:
							return WinOptions.Player1Win;
					}
					break;
				case PlayOptions.Scissors:
					switch (player2) {
						case PlayOptions.Rock:
							return WinOptions.Player2Win;
						case PlayOptions.Paper:
							return WinOptions.Player1Win;
					}
					break;
			}
		},

		playAgain() {
			this.ai_choice = 0;
			this.resultShow = false;
			this.win = WinOptions.Draw;
			this.winCount = 0;
			this.userPicked = 0;
		},
	},
	data() {
		return {
			ai_choice: 0,
			resultShow: false,
			win: WinOptions.Draw,
			winCount: 0,
			userPicked: 0,
		};
	},
};
</script>


<style scoped>
.hoverEffect:hover {
	cursor: pointer;
	transform: scale(1.2);
	box-shadow: 5px 20px 30px rgb(0 0 0 / 5%) !important;
}

/* Animations */
.wipe-open-enter-active {
	transition: all 0.8s ease;
}

.wipe-open-leave-active {
	transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.wipe-open-enter-from {
	opacity: 0;
	transform: scale(0);
}

.wipe-open-leave-to {
	opacity: 0;
	transform: translateY(100px);
}
</style>
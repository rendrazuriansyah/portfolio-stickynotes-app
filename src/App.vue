<script setup>
import { ref } from "vue";
const showForm = ref(false);
const newMemo = ref("");
const memos = ref([]);
const errorMessage = ref("");

/**
 * Adds a new memo to the memos array if the newMemo value is not empty.
 * Sets the errorMessage value to "Please enter a memo" if the newMemo value is empty.
 * Resets the newMemo value and showForm value to their default values.
 */
function addMemo() {
	if (!newMemo.value) {
		errorMessage.value = "Please enter a memo";
		return;
	}
	memos.value.push({
		id: Date.now(),
		memo: newMemo.value,
		date: new Date().toLocaleString("en-GB"),
		backgroundColor: getRandomColor(),
	});
	newMemo.value = "";
	showForm.value = false;
}

/**
 * Deletes a memo from the memos array based on the provided id.
 * The id of the memo to be deleted.
 * This function does not return anything.
 */
function deleteMemo(id) {
	memos.value = memos.value.filter((memo) => memo.id !== id);
}

/**
 * Returns a random color from the list of memo colors.
 */
function getRandomColor() {
	const memoColors = [
		"#FFEB3B", // Yellow
		"#FFCDD2", // Light Red
		"#C8E6C9", // Light Green
		"#BBDEFB", // Light Blue
		"#FFECB3", // Light Orange
		"#E1BEE7", // Light Purple
		"#F8BBD0", // Light Pink
		"#B2EBF2", // Light Cyan
		"#DCEDC8", // Light Lime
		"#F5F5F5", // Light Grey
	];
	return memoColors[Math.floor(Math.random() * memoColors.length)];
}
</script>

<template>
	<main>
		<div class="container">
			<header>
				<h1 class="header-title">Sticky Notes</h1>
				<button @click="showForm = true" class="header-button">
					+
				</button>
			</header>
			<div class="card-container">
				<div
					v-for="memo in memos"
					class="card"
					:key="memo.id"
					:style="{ backgroundColor: memo.backgroundColor }"
				>
					<p class="card-content">{{ memo.memo }}</p>
					<div class="card-footer">
						<p class="card-date">{{ memo.date }}</p>
						<button
							@click="deleteMemo(memo.id)"
							class="card-delete-btn"
						>
							<svg
								xmlns="http://www.w3.org/2000/svg"
								viewBox="0 0 448 512"
							>
								<path
									d="M170.5 51.6L151.5 80l145 0-19-28.4c-1.5-2.2-4-3.6-6.7-3.6l-93.7 0c-2.7 0-5.2 1.3-6.7 3.6zm147-26.6L354.2 80 368 80l48 0 8 0c13.3 0 24 10.7 24 24s-10.7 24-24 24l-8 0 0 304c0 44.2-35.8 80-80 80l-224 0c-44.2 0-80-35.8-80-80l0-304-8 0c-13.3 0-24-10.7-24-24S10.7 80 24 80l8 0 48 0 13.8 0 36.7-55.1C140.9 9.4 158.4 0 177.1 0l93.7 0c18.7 0 36.2 9.4 46.6 24.9zM80 128l0 304c0 17.7 14.3 32 32 32l224 0c17.7 0 32-14.3 32-32l0-304L80 128zm80 64l0 208c0 8.8-7.2 16-16 16s-16-7.2-16-16l0-208c0-8.8 7.2-16 16-16s16 7.2 16 16zm80 0l0 208c0 8.8-7.2 16-16 16s-16-7.2-16-16l0-208c0-8.8 7.2-16 16-16s16 7.2 16 16zm80 0l0 208c0 8.8-7.2 16-16 16s-16-7.2-16-16l0-208c0-8.8 7.2-16 16-16s16 7.2 16 16z"
								/>
							</svg>
						</button>
					</div>
				</div>
			</div>
		</div>
		<div v-if="showForm" class="form-overlay">
			<div class="form-modal">
				<button
					@click="
						showForm = false;
						errorMessage = '';
					"
					class="form-close-btn"
				>
					&times;
				</button>
				<p v-show="!newMemo" class="form-error">
					{{ errorMessage }}
				</p>
				<textarea
					v-model="newMemo"
					class="form-textarea-memo"
					name="memo"
					id="memo"
					cols="30"
					rows="10"
				>
				</textarea>
				<button @click="addMemo" class="form-save-btn">Save</button>
			</div>
		</div>
		<footer>
			<p>&copy; 2024 Rendra Zuriansyah</p>
		</footer>
	</main>
</template>

<style scoped>
* {
	font-family: "Lato", sans-serif;
}

main {
	height: 100vh;
	/* width: 100vw; */
}

.container {
	max-width: 900px;
	padding: 10px;
	margin: 0 auto;
}

header {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.header-title {
	font-size: 48px;
	font-weight: bold;
	margin-bottom: 25px;
	color: #212121;
}
.header-button {
	font-size: 30px;
	border: none;
	padding: 10px;
	width: 50px;
	height: 50px;
	cursor: pointer;
	border-radius: 100%;
	background-color: #fdd835;
	color: #212121;
	display: flex;
	align-items: center;
	justify-content: center;
}

.card-container {
	display: flex;
	flex-wrap: wrap;
	gap: 20px;
	margin-top: 20px;
	justify-content: space-evenly;
	margin-bottom: 60px;
}

.card {
	width: 300px;
	height: 200px;
	padding: 10px 15px;
	background-color: #fdd835;
	margin-bottom: 20px;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	border-radius: 15px;
	box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
}

.card-footer {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.card-delete-btn {
	font-size: 25px;
	border: none;
	padding: 10px;
	width: 40px;
	height: 40px;
	cursor: pointer;
	border-radius: 25%;
	background-color: #ff6f61;
	color: #212121;
	display: flex;
	align-items: center;
	justify-content: center;
	box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
}

.form-overlay {
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	background-color: rgba(0, 0, 0, 0.77);
	z-index: 10;
	display: flex;
	align-items: center;
	justify-content: center;
}

.form-modal {
	width: 420px;
	background-color: white;
	border-radius: 10px;
	padding: 30px;
	position: relative;
	display: flex;
	flex-direction: column;
}

.form-save-btn {
	padding: 10px 20px;
	font-size: 20px;
	width: 100%;
	background-color: #fdd835;
	border: none;
	cursor: pointer;
	border-radius: 10px;
	margin-top: 20px;
	color: #212121;
	font-weight: bold;
}

.form-close-btn {
	position: absolute;
	top: 5px;
	right: 5px;
	width: 30px;
	height: 30px;
	background-color: transparent;
	border: none;
	font-size: 30px;
	cursor: pointer;
}

.form-textarea-memo {
	height: 200px;
	resize: none;
	font-size: 18px;
	padding: 10px;
	border: 1px solid #fdd835;
	border-radius: 10px;
	outline: none;
	margin-top: 10px;
}

.form-textarea-memo:focus {
	border: 5px solid #fdd835;
}

.form-error {
	color: red;
	margin: 10px 0 0 0;
}

footer {
	position: fixed;
	bottom: 0;
	left: 0;
	width: 100%;
	height: 50px;
	background-color: #fdd835;
	display: flex;
	align-items: center;
	justify-content: center;
	color: #212121;
	box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
}
</style>

<script>


	let questions = [];

	function startQuiz() {
		const category = document.getElementById('category').value;
		const difficulty = document.getElementById('difficulty').value;


		const apiUrl = `https://opentdb.com/api.php?amount=10&category=${category}&difficulty=${difficulty}&type=multiple`;
        console.log(apiUrl);

        fetch(apiUrl)
            .then((response) => response.json())
            .then((data) => {
                questions = data.results;
            });
	}


	let correctAnswers = 0;
	let incorrectAnswers = 0;
	let unanswered = 0;

	function submitTrivia() {

		for (let i = 0; i < questions.length; i++) {
			let question = questions[i].question;
			let correctAnswer = questions[i].correct_answer;
			let incorrectAnswers = questions[i].incorrect_answers;
			let userAnswer = document.querySelector(`input[name="${question}"]:checked`).value;


			if (userAnswer === correctAnswer) {
				correctAnswers++;
			} else if (userAnswer === undefined) {
				unanswered++;
			} else {
				incorrectAnswers++;
			}
		}
	}
</script>
<div
></div>

<div>
	<form id="quiz-form">

		<label for="category">Category:</label>
		<select id="category" name="category">
			<option value="9">General Knowledge</option>
			<option value="10">Books</option>
			<option value="11">Film</option>
			<option value="12">Music</option>
            <option value="13">Musicals & Theatres</option>
			<!-- Add more options for other categories -->
		</select><br />

		<label for="difficulty">Difficulty:</label>
		<select id="difficulty" name="difficulty">
			<option value="easy">Easy</option>
			<option value="medium">Medium</option>
			<option value="hard">Hard</option>
		</select><br />


		<button on:click={startQuiz}>Start Quiz</button>
	</form>
</div>

<div>
	<form action="triviaQuestions">
		{#each questions as question}
			<p>{@html question.question}</p>
			<input
				type="radio"
				name={question.question}
				value={question.correct_answer}
				id="correctAnswer"
			/>{@html question.correct_answer}<br />
			{#each question.incorrect_answers as incorrect}
				<input
					type="radio"
					name={question.question}
					value={incorrect}
					id="IncorrectAnswer"
				/>{@html incorrect}<br />
			{/each}
		{/each}

		<button on:click={submitTrivia}>Submit</button>
	</form>

	<div>
		<p>Correct Answers: {correctAnswers}</p>
		<p>Incorrect Answers: {incorrectAnswers}</p>
	</div>
</div>

<style>
    .triviaApp {
        padding: 2%;
    }
    
    label {
        padding: 10px;
    }
    select {
        margin: 0 10px 10px;
        padding: 10px;
    }
    button {
        padding: 10px;
        margin: 10px 0 10px 0;
    }
</style>
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>관용어 퀴즈</title>
<style>
  body {
    font-family: 'Arial', sans-serif;
    max-width: 600px;
    margin: 30px auto;
    padding: 0 20px;
    background: #f9f9f9;
    color: #222;
  }
  h1, h2 {
    text-align: center;
    color: #2c3e50;
  }
  .btn {
    display: inline-block;
    background: #3498db;
    color: white;
    padding: 10px 25px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
    margin: 20px auto;
    transition: background-color 0.3s;
  }
  .btn:hover {
    background: #2980b9;
  }
  .quiz-container {
    background: white;
    border-radius: 8px;
    padding: 25px 30px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  }
  .question {
    font-size: 1.2rem;
    margin-bottom: 15px;
  }
  label {
    display: block;
    margin: 8px 0;
    cursor: pointer;
    padding: 7px 12px;
    border-radius: 4px;
    border: 1px solid transparent;
    transition: background-color 0.3s, border-color 0.3s;
  }
  input[type="radio"] {
    margin-right: 10px;
  }
  label.correct {
    background-color: #d4edda;
    border-color: #28a745;
    color: #155724;
    font-weight: bold;
  }
  label.incorrect {
    background-color: #f8d7da;
    border-color: #dc3545;
    color: #721c24;
  }
  .navigation {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
  }
  .score-section {
    text-align: center;
  }
  .explanation {
    font-size: 0.9rem;
    margin-top: 10px;
    color: #555;
    font-style: italic;
  }
</style>
</head>
<body>

<h1>관용어 퀴즈</h1>

<div id="start-screen" class="quiz-container">
  <h2>퀴즈에 오신 것을 환영합니다!</h2>
  <button id="start-btn" class="btn">퀴즈 시작하기</button>
</div>

<div id="quiz-screen" class="quiz-container" style="display:none;">
  <div id="question-container">
    <div class="question" id="question-text"></div>
    <form id="answers-form"></form>
    <div class="explanation" id="explanation" style="display:none;"></div>
  </div>

  <div class="navigation">
    <button id="prev-btn" class="btn" style="visibility:hidden;">이전</button>
    <button id="next-btn" class="btn" disabled>다음</button>
  </div>
</div>

<div id="result-screen" class="quiz-container" style="display:none;">
  <h2>퀴즈 완료!</h2>
  <p id="score-text"></p>
  <button id="restart-btn" class="btn">다시 풀기</button>
</div>

<script>
  // 문제 데이터
  const quizData = [
    {
      question: "1. '손을 씻다'의 의미는?",
      choices: [
        "책임을 피하다",
        "깨끗이 청소하다",
        "더 이상 관여하지 않다",
        "손을 씻는 행위"
      ],
      answer: 2,
      explanation: "‘손을 씻다’는 더 이상 어떤 일에 관여하지 않는다는 뜻입니다."
    },
    {
      question: "2. '눈에 띄다'의 뜻은?",
      choices: [
        "눈이 아프다",
        "특별히 잘 보이다",
        "사람을 속이다",
        "눈물을 흘리다"
      ],
      answer: 1,
      explanation: "‘눈에 띄다’는 특별히 잘 보여서 사람들의 관심을 끈다는 의미입니다."
    },
    {
      question: "3. '발 벗고 나서다'의 뜻은?",
      choices: [
        "적극적으로 돕다",
        "신발을 벗다",
        "도망가다",
        "조용히 있다"
      ],
      answer: 0,
      explanation: "‘발 벗고 나서다’는 적극적으로 나서서 돕는다는 뜻입니다."
    },
    {
      question: "4. '입에 침이 마르다'의 의미는?",
      choices: [
        "말을 많이 하다",
        "입이 마르다",
        "음식을 먹지 않다",
        "물을 마시지 않다"
      ],
      answer: 0,
      explanation: "‘입에 침이 마르다’는 어떤 말을 아주 많이 반복해서 한다는 뜻입니다."
    },
    {
      question: "5. '손바닥 뒤집듯'의 뜻은?",
      choices: [
        "빠르게 움직이다",
        "쉽게 바뀌다",
        "손을 뒤집다",
        "무시하다"
      ],
      answer: 1,
      explanation: "‘손바닥 뒤집듯’은 매우 쉽게 바뀌거나 변한다는 뜻입니다."
    },
    {
      question: "6. '하늘의 별 따기'란?",
      choices: [
        "불가능한 일",
        "별을 따는 일",
        "높은 곳으로 가기",
        "희망찬 미래"
      ],
      answer: 0,
      explanation: "‘하늘의 별 따기’는 매우 어렵거나 불가능한 일을 의미합니다."
    },
    {
      question: "7. '등잔 밑이 어둡다'의 의미는?",
      choices: [
        "가까운 곳이 오히려 잘 보이지 않는다",
        "등잔이 고장났다",
        "어둠 속에서 걷다",
        "밑을 내려다본다"
      ],
      answer: 0,
      explanation: "‘등잔 밑이 어둡다’는 가까운 곳에 있는 것이 오히려 잘 보이지 않는다는 뜻입니다."
    },
    {
      question: "8. '바늘 도둑이 소 도둑 된다'란?",
      choices: [
        "작은 잘못이 큰 잘못으로 이어진다",
        "도둑이 많다",
        "소가 바늘을 훔친다",
        "작은 것만 훔친다"
      ],
      answer: 0,
      explanation: "‘바늘 도둑이 소 도둑 된다’는 작은 잘못이 나중에 큰 잘못으로 이어진다는 의미입니다."
    },
    {
      question: "9. '가는 말이 고와야 오는 말이 곱다'란?",
      choices: [
        "상대방에게 친절해야 좋은 말을 듣는다",
        "말을 잘 타야 한다",
        "길이 아름답다",
        "예쁜 말을 해야 한다"
      ],
      answer: 0,
      explanation: "‘가는 말이 고와야 오는 말이 곱다’는 상대에게 좋은 말을 해야 좋은 말을 듣는다는 뜻입니다."
    },
    {
      question: "10. '귀가 얇다'의 의미는?",
      choices: [
        "말을 잘 듣는다",
        "잘 속는다",
        "귀가 크다",
        "잘 듣지 않는다"
      ],
      answer: 1,
      explanation: "‘귀가 얇다’는 쉽게 남의 말을 믿거나 잘 속는다는 의미입니다."
    }
  ];

  let currentQuestionIndex = 0;
  let userAnswers = Array(quizData.length).fill(null);

  const startScreen = document.getElementById('start-screen');
  const quizScreen = document.getElementById('quiz-screen');
  const resultScreen = document.getElementById('result-screen');

  const questionText = document.getElementById('question-text');
  const answersForm = document.getElementById('answers-form');
  const explanationDiv = document.getElementById('explanation');

  const prevBtn = document.getElementById('prev-btn');
  const nextBtn = document.getElementById('next-btn');
  const startBtn = document.getElementById('start-btn');
  const restartBtn = document.getElementById('restart-btn');
  const scoreText = document.getElementById('score-text');

  // 시작 버튼 클릭
  startBtn.addEventListener('click', () => {
    startScreen.style.display = 'none';
    quizScreen.style.display = 'block';
    currentQuestionIndex = 0;
    userAnswers.fill(null);
    showQuestion();
  });

  // 재시작 버튼 클릭
  restartBtn.addEventListener('click', () => {
    resultScreen.style.display = 'none';
    startScreen.style.display = 'block';
  });

  // 이전 버튼 클릭
  prevBtn.addEventListener('click', () => {
    if (currentQuestionIndex > 0) {
      currentQuestionIndex--;
      showQuestion();
    }
  });

  // 다음 버튼 클릭
  nextBtn.addEventListener('click', () => {
    // 답이 선택됐으면 저장
    const selected = answersForm.querySelector('input[name="answer"]:checked');
    if (!selected) return; // 버튼 비활성화로 막음

    userAnswers[currentQuestionIndex] = parseInt(selected.value);

    if (currentQuestionIndex < quizData.length - 1) {
      currentQuestionIndex++;
      showQuestion();
    } else {
      // 마지막 문제면 결과 보여주기
      showResult();
    }
  });

  // 문제 보여주는 함수
  function showQuestion() {
    explanationDiv.style.display = 'none';
    nextBtn.disabled = true;

    const q = quizData[currentQuestionIndex];
    questionText.textContent = q.question;

    // 이전 버튼 visibility
    prevBtn.style.visibility = currentQuestionIndex === 0 ? 'hidden' : 'visible';

    // 선택지 생성
    answersForm.innerHTML = '';
    q.choices.forEach((choice, idx) => {
      const label = document.createElement('label');
      label.htmlFor = 'answer' + idx;
      label.textContent = choice;
      label.className = '';

      const input = document.createElement('input');
      input.type = 'radio';
      input.name = 'answer';
      input.id = 'answer' + idx;
      input.value = idx;
      input.disabled = false;

      // 선택된 답 있으면 체크해두기
      if (userAnswers[currentQuestionIndex] === idx) {
        input.checked = true;
        nextBtn.disabled = false;
      }

      // 선택하면 즉시 정답/오답 표시
      input.addEventListener('change', () => {
        nextBtn.disabled = false;
        showAnswerFeedback(idx, q.answer);
      });

      label.prepend(input);
      answersForm.appendChild(label);
    });

    // 이미 선택한 답이 있으면 정답/오답 색상 표시
    if (userAnswers[currentQuestionIndex] !== null) {
      showAnswerFeedback(userAnswers[currentQuestionIndex], q.answer);
    }
  }

  // 답 선택 후 정답/오답 표시 및 해설 보여주기
  function showAnswerFeedback(selectedIdx, correctIdx) {
    const labels = answersForm.querySelectorAll('label');
    labels.forEach((label, idx) => {
      label.classList.remove('correct', 'incorrect');
      label.querySelector('input').disabled = true;
      if (idx === correctIdx) {
        label.classList.add('correct');
      }
      if (idx === selectedIdx && selectedIdx !== correctIdx) {
        label.classList.add('incorrect');
      }
    });

    explanationDiv.textContent = quizData[currentQuestionIndex].explanation;
    explanationDiv.style.display = 'block';
  }

  // 결과 보여주는 함수
  function showResult() {
    quizScreen.style.display = 'none';
    resultScreen.style.display = 'block';

    let score = 0;
    for (let i = 0; i < quizData.length; i++) {
      if (userAnswers[i] === quizData[i].answer) score++;
    }
    scoreText.textContent = `당신의 점수: ${score} / ${quizData.length}`;
  }
</script>

</body>
</html>

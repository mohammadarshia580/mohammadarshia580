<!DOCTYPE html><html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>موسسه پیشگامان در راه موفقیت</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; }
        .hidden { display: none; }
    </style>
</head>
<body>
    <div id="login">
        <h2>ورود به آزمون</h2>
        <input type="text" id="username" placeholder="نام کاربری">
        <input type="password" id="password" placeholder="رمز عبور">
        <button onclick="login()">ورود</button>
    </div><div id="quiz" class="hidden">
    <h2 id="questionText"></h2>
    <button id="opt1" onclick="submitQuiz(0)"></button>
    <button id="opt2" onclick="submitQuiz(1)"></button>
    <button id="opt3" onclick="submitQuiz(2)"></button>
    <p id="result"></p>
    <h3 id="scoreInfo" class="hidden"></h3>
</div>

<script>
    let users = JSON.parse(localStorage.getItem("users")) || {"محمد": "1234", "عرشیا": "5678"};
    let questions = JSON.parse(localStorage.getItem("questions")) || [
        {question: "پایتخت ایران کجاست؟", options: ["تهران", "اصفهان", "مشهد"], answer: 0}
    ];
    let scores = JSON.parse(localStorage.getItem("scores")) || {};
    let currentUser = "";
    let correctAnswers = 0;

    function login() {
        let username = document.getElementById("username").value;
        let password = document.getElementById("password").value;
        
        if (users[username] && users[username] === password) {
            currentUser = username;
            document.getElementById("login").classList.add("hidden");
            document.getElementById("quiz").classList.remove("hidden");
            loadQuestion();
        } else {
            alert("نام کاربری یا رمز عبور اشتباه است.");
        }
    }

    function loadQuestion() {
        if (questions.length === 0) {
            document.getElementById("quiz").innerHTML = "<h2>هیچ سوالی موجود نیست.</h2>";
            return;
        }
        let q = questions[0];
        document.getElementById("questionText").innerText = q.question;
        document.getElementById("opt1").innerText = q.options[0];
        document.getElementById("opt2").innerText = q.options[1];
        document.getElementById("opt3").innerText = q.options[2];
    }

    function submitQuiz(choice) {
        if (questions.length > 0 && choice === questions[0].answer) {
            correctAnswers++;
        }
        
        let percentage = (correctAnswers / questions.length) * 100;
        let tazr = Math.round(percentage * 10);
        
        scores[currentUser] = percentage;
        localStorage.setItem("scores", JSON.stringify(scores));
        
        let sortedScores = Object.values(scores).sort((a, b) => b - a);
        let rank = sortedScores.indexOf(percentage) + 1;
        
        document.getElementById("result").innerText = "آزمون پایان یافت.";
        document.getElementById("scoreInfo").innerText = `درصد: ${percentage.toFixed(2)}% \n تراز: ${tazr} \n رتبه شما: ${rank} از ${sortedScores.length}`;
        document.getElementById("scoreInfo").classList.remove("hidden");
    }
</script>

</body>
</html>

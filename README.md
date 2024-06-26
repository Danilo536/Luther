<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }

        .Text {
            font-size: 21px;
        }

        .navbar {
            background-color: #333;
            color: #fff;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 10px; /* Anpassung des Paddings */
            width: 100%; /* 100% Breite */
            height: 60px; /* Höhe reduziert */
        }

        .navbar-title {
            margin-left: 20px;
            font-weight: normal;
            color: white;
            font-size: 30px; /* Größe des Navbar-Titels auf 30px */
        }

        .back-button {
            margin-right: 25px;
            font-weight: normal;
            color: white;
            font-size: 15px; /* Größe des Zurück-Buttons auf 15px */
            text-decoration: none;
            display: inline-block; /* Zurück-Button standardmäßig anzeigen */
        }

        #container {
            background-color: white;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        button {
            padding: 15px 30px; /* Größeres Padding */
            font-size: 18px; /* Größere Schrift */
            cursor: pointer;
            background-color: black;
            color: white;
            border: none;
            margin-top: 20px; /* Größerer Abstand zur letzten Frage */
        }

        button:hover {
            background-color: #444;
        }

        .antwort {
            font-size: 18px; /* Größere Schrift für Antworten */
            cursor: pointer; /* Cursor auf Zeiger ändern */
        }

        .ergebnis {
            font-size: 19px; /* Schriftgröße für Auswertung */
        }

        .richtig {
            color: green; /* Farbe für richtige Antworten */
        }

        .falsch {
            color: red; /* Farbe für falsche Antworten */
        }
    </style>
</head>
<body>
    <div class="navbar" id="navbar">
        <div class="navbar-title">Martin Luther</div>
        <a href="index.html" class="back-button" id="backButton">Zurück</a>
    </div>

    <div id="container">
        <div id="start">
            <h1>Willkommen zum Quiz!</h1>
            <p>Testen Sie Ihr Wissen mit unserem spannenden Quiz. Klicken Sie auf den Button unten, um das Quiz zu starten.</p>
            <button onclick="startQuiz()">Quiz starten</button>
            <p style="color: red;">Warnung: Wenn Sie das Quiz starten, können Sie erst zurück, wenn Sie das Quiz abgeschlossen haben.</p>
        </div>
        
        <div id="quiz" style="display:none;">
            <form id="quizForm">
                <!-- Frage 1 -->
                <h2>Frage 1: Was ist die Hauptstadt von Deutschland?</h2>
                <label><input type="radio" name="q1" value="Berlin" onclick="selectAnswer(this)"> <span class="antwort" onclick="selectAnswer(this)">Berlin</span></label><br>
                <label><input type="radio" name="q1" value="München" onclick="selectAnswer(this)"> <span class="antwort" onclick="selectAnswer(this)">München</span></label><br>
                <label><input type="radio" name="q1" value="Hamburg" onclick="selectAnswer(this)"> <span class="antwort" onclick="selectAnswer(this)">Hamburg</span></label><br>

                <!-- Frage 2 -->
                <h2>Frage 2: Welche Farbe hat die Sonne?</h2>
                <label><input type="radio" name="q2" value="Gelb" onclick="selectAnswer(this)"> <span class="antwort">Gelb</span></label><br>
                <label><input type="radio" name="q2" value="Blau" onclick="selectAnswer(this)"> <span class="antwort">Blau</span></label><br>
                <label><input type="radio" name="q2" value="Grün" onclick="selectAnswer(this)"> <span class="antwort">Grün</span></label><br>

                <!-- Frage 3 -->
                <h2>Frage 3: Wie viele Kontinente gibt es?</h2>
                <label><input type="radio" name="q3" value="5" onclick="selectAnswer(this)"> <span class="antwort">5</span></label><br>
                <label><input type="radio" name="q3" value="6" onclick="selectAnswer(this)"> <span class="antwort">6</span></label><br>
                <label><input type="radio" name="q3" value="7" onclick="selectAnswer(this)"> <span class="antwort">7</span></label><br>

                <button type="button" onclick="auswerten()">Quiz auswerten</button>
            </form>

            <p id="result" class="ergebnis"></p>

            <!-- Buttons nach der Auswertung -->
            <div id="buttonContainer" style="display: none;">
                <button onclick="location.reload()">Quiz neustarten</button>
                <a href="index.html"><button>Zurück zur Startseite</button></a>
            </div>
        </div>
    </div>

    <script>
        let quizAusgewertet = false; // Variable für den Zustand des Quiz

        function startQuiz() {
            document.getElementById('start').style.display = 'none';
            document.getElementById('quiz').style.display = 'block';
            document.getElementById('backButton').style.display = 'none'; // Zurück-Button ausblenden
        }

        function selectAnswer(element) {
            if (!quizAusgewertet) { // Nur wenn das Quiz nicht ausgewertet wurde
                // Markiert das zugehörige radio input Element
                element.previousElementSibling.checked = true;
            }
        }

        function auswerten() {
            quizAusgewertet = true; // Setzt den Zustand des Quiz auf ausgewertet

            // Die richtigen Antworten
            const richtigeAntworten = {
                q1: "Berlin",
                q2: "Gelb",
                q3: "7"
            };

            // Punktestand initialisieren
            let punktestand = 0;

            // Antworten aus dem Formular auslesen
            const form = document.getElementById('quizForm');
            const formData = new FormData(form);

            // Über alle Fragen iterieren
            for (let [frage, antwort] of formData.entries()) {
                const frageElement = form.querySelector(`[name=${frage}]`);
                const antwortElement = frageElement.nextElementSibling.querySelector('.antwort');

                // Antworten hervorheben
                if (antwort === richtigeAntworten[frage]) {
                    antwortElement.classList.add('richtig');
                    punktestand++;
                } else {
                    antwortElement.classList.add('falsch');
                }
            }

            // Ergebnis anzeigen
            const result = document.getElementById('result');
            result.textContent = `Du hast ${punktestand} von ${Object.keys(richtigeAntworten).length} Fragen richtig beantwortet.`;
            result.className = 'ergebnis'; // CSS-Klasse für größere Schrift hinzufügen

            document.getElementById('backButton').style.display = 'inline-block'; // Zurück-Button wieder anzeigen

            // Buttons für Quiz-Neustart und Zurück zur Startseite anzeigen
            document.getElementById('buttonContainer').style.display = 'block';
        }
    </script>
</body>
</html>



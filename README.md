# Fix-Chemistry-Batch-name<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Simple Batch Name Voting</title>
<style>
  body {
    font-family: Arial, sans-serif;
    max-width: 450px;
    margin: 40px auto;
    background: #f7f7f7;
    padding: 20px;
    border-radius: 8px;
  }
  h1 {
    text-align: center;
    color: #333;
  }
  label {
    display: block;
    margin-top: 15px;
    font-weight: bold;
  }
  input, select {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    border-radius: 5px;
    border: 1px solid #aaa;
  }
  button {
    margin-top: 20px;
    width: 100%;
    background: #007bff;
    color: white;
    font-weight: bold;
    padding: 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  button:hover {
    background: #0056b3;
  }
  #message {
    margin-top: 20px;
    font-weight: bold;
    text-align: center;
  }
  #results {
    margin-top: 30px;
    background: white;
    padding: 15px;
    border-radius: 8px;
  }
  #results h2 {
    text-align: center;
  }
  #results ul {
    list-style: none;
    padding-left: 0;
  }
  #results li {
    padding: 6px 0;
    border-bottom: 1px solid #ddd;
  }
</style>
</head>
<body>

<h1>Batch Name Voting</h1>

<form id="voteForm">
  <label for="name">Your Name</label>
  <input type="text" id="name" required placeholder="Enter your name" />

  <label for="roll">Roll Number</label>
  <input type="number" id="roll" required placeholder="Enter your roll number" min="1" />

  <label for="batch">Select Batch Name</label>
  <select id="batch" required>
    <option value="" disabled selected>Select your batch name</option>
    <option>The Alkali Syndicate</option>
    <option>Chemistry Freak</option>
    <option>Redox Rebels</option>
    <option>Periodic Squad</option>
    <option>Acid Avengers</option>
    <option>Catalyst Crew</option>
    <option>Hydronium Heros</option>
    <option>Alchemists</option>
    <option>Rock salt</option>
    <option>Electron Empire</option>
    <option>Proton Pioneer's</option>
    <option>H₂O-holics</option>
    <option>DDT</option>
    <option>Toxic Orbital</option>
    <option>RDX</option>
    <option>Covalent Soul's</option>
  </select>

  <button type="submit">Submit Vote</button>
</form>

<div id="message"></div>

<div id="results">
  <h2>Current Voting Results</h2>
  <ul id="resultsList"></ul>
</div>

<script>
  const form = document.getElementById('voteForm');
  const message = document.getElementById('message');
  const resultsList = document.getElementById('resultsList');

  // Load votes from localStorage or empty
  const votes = JSON.parse(localStorage.getItem('votes') || '{}');
  // Track roll numbers who voted
  const voters = JSON.parse(localStorage.getItem('voters') || '{}');

  function renderResults() {
    resultsList.innerHTML = '';
    if (Object.keys(votes).length === 0) {
      resultsList.innerHTML = '<li>No votes yet.</li>';
      return;
    }
    for (const [batch, count] of Object.entries(votes)) {
      const li = document.createElement('li');
      li.textContent = `${batch}: ${count} vote${count > 1 ? 's' : ''}`;
      resultsList.appendChild(li);
    }
  }

  form.addEventListener('submit', e => {
    e.preventDefault();

    const name = form.name.value.trim();
    const roll = form.roll.value.trim();
    const batch = form.batch.value;

    if (!name || !roll || !batch) {
      message.style.color = 'red';
      message.textContent = 'Please fill in all fields.';
      return;
    }

    if (voters[roll]) {
      message.style.color = 'red';
      message.textContent = 'This roll number has already voted.';
      return;
    }

    // Save vote
    votes[batch] = (votes[batch] || 0) + 1;
    voters[roll] = true;

    localStorage.setItem('votes', JSON.stringify(votes));
    localStorage.setItem('voters', JSON.stringify(voters));

    message.style.color = 'green';
    message.textContent = `Thank you, ${name}! Your vote for "${batch}" is recorded.`;

    form.reset();
    renderResults();
  });

  renderResults();
</script>

</body>
</html>

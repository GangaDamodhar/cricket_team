// Import required packages
const express = require("express");
const sqlite3 = require("sqlite3");
const bodyParser = require("body-parser");

// Create an Express instance
const app = express();

// Create a database connection
const db = new sqlite3.Database("cricketTeam.db");

// Middleware to parse JSON requests
app.use(bodyParser.json());

// API 1: Get all players
app.get("/players", (req, res) => {
  db.all("SELECT * FROM cricket_team", (err, rows) => {
    if (err) {
      res.status(500).json({ error: err.message });
      return;
    }
    res.status(200).json(rows); // Modified response format
  });
});

// API 2: Create a new player
app.post("/players", (req, res) => {
  const { player_name, jersey_number, role } = req.body;
  if (!player_name || !jersey_number || !role) {
    res.status(400).json({ error: "Missing required fields" });
    return;
  }

  db.run(
    "INSERT INTO cricket_team (player_name, jersey_number, role) VALUES (?, ?, ?)",
    [player_name, jersey_number, role],
    function (err) {
      if (err) {
        res.status(500).json({ error: err.message });
        return;
      }
      res.status(201).json({ message: "Player Added to Team" });
    }
  );
});
// API 3: Get a player by playerId
app.get("/players/:playerId", (req, res) => {
  const playerId = req.params.playerId;
  db.get(
    "SELECT * FROM cricket_team WHERE player_id = ?",
    [playerId],
    (err, row) => {
      if (err) {
        res.status(500).json({ error: err.message });
        return;
      }
      if (!row) {
        res.status(404).json({ error: "Player not found" });
        return;
      }
      res.status(200).json(row); // Modified response format
    }
  );
});

// API 4: Update a player by playerId
app.put("/players/:playerId", (req, res) => {
  const playerId = req.params.playerId;
  const { player_name, jersey_number, role } = req.body;
  if (!player_name || !jersey_number || !role) {
    res.status(400).json({ error: "Missing required fields" });
    return;
  }

  db.run(
    "UPDATE cricket_team SET player_name = ?, jersey_number = ?, role = ? WHERE player_id = ?",
    [player_name, jersey_number, role, playerId],
    function (err) {
      if (err) {
        res.status(500).json({ error: err.message });
        return;
      }
      res.status(200).json({ message: "Player Details Updated" });
    }
  );
});

// API 5: Delete a player by playerId
app.delete("/players/:playerId", (req, res) => {
  const playerId = req.params.playerId;
  db.run("DELETE FROM cricket_team WHERE player_id = ?", [playerId], (err) => {
    if (err) {
      res.status(500).json({ error: err.message });
      return;
    }
    res.status(200).json({ message: "Player Removed" });
  });
});

// Start the Express server
const port = process.env.PORT || 3000;
app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});

// Export the Express instance
module.exports = app;

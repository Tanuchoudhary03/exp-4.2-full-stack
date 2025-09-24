// server.js
const express = require("express");
const cors = require("cors");
const app = express();
const PORT = 5000;

app.use(cors());
app.use(express.json());

let cards = [
  { id: 1, suit: "Hearts", value: "A" },
  { id: 2, suit: "Spades", value: "K" },
  { id: 3, suit: "Diamonds", value: "10" }
];

// GET all cards
app.get("/cards", (req, res) => {
  res.json(cards);
});

// GET card by ID
app.get("/cards/:id", (req, res) => {
  const card = cards.find(c => c.id === parseInt(req.params.id));
  card ? res.json(card) : res.status(404).json({ message: "Card not found" });
});

// POST new card
app.post("/cards", (req, res) => {
  const { suit, value } = req.body;
  const newCard = { id: cards.length + 1, suit, value };
  cards.push(newCard);
  res.status(201).json(newCard);
});

// PUT update card
app.put("/cards/:id", (req, res) => {
  const card = cards.find(c => c.id === parseInt(req.params.id));
  if (!card) return res.status(404).json({ message: "Card not found" });

  const { suit, value } = req.body;
  if (suit) card.suit = suit;
  if (value) card.value = value;

  res.json(card);
});

// DELETE card
app.delete("/cards/:id", (req, res) => {
  const index = cards.findIndex(c => c.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ message: "Card not found" });

  const deleted = cards.splice(index, 1);
  res.json(deleted);
});

// Start server
app.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));

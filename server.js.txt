const express = require("express");
const { WebSocketServer } = require("ws");
const http = require("http");

const app = express();
const server = http.createServer(app);
const wss = new WebSocketServer({ server });

wss.on("connection", ws => {
  console.log("ESP32 connected!");
  ws.on("message", msg => {
    console.log("Received from ESP32:", msg.toString());
    ws.send("Data received OK!");
  });
});

app.get("/", (req, res) => {
  res.send("ESP32 WebSocket Server Running!");
});

const PORT = process.env.PORT || 10000;
server.listen(PORT, () => console.log(`Server running on port ${PORT}`));

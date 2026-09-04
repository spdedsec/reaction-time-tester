# reaction-time-tester

1. Create the project
npx create-next-app@latest reaction-time
cd reaction-time
npm run dev

Choose App Router when prompted.


2. app/page.js

Replace the contents with:

"use client";

import { useEffect, useRef, useState } from "react";

export default function Home() {
  const [gameState, setGameState] = useState("idle");
  const [reactionTime, setReactionTime] = useState(null);

  const startTime = useRef(null);
  const timer = useRef(null);

  const startGame = () => {
    setReactionTime(null);
    setGameState("waiting");

    const delay = Math.floor(Math.random() * 3000) + 2000;

    timer.current = setTimeout(() => {
      startTime.current = performance.now();
      setGameState("ready");
    }, delay);
  };

  const handleClick = () => {
    if (gameState === "idle") {
      startGame();
      return;
    }

    if (gameState === "waiting") {
      clearTimeout(timer.current);
      setGameState("too-early");
      return;
    }

    if (gameState === "ready") {
      const endTime = performance.now();
      const time = Math.round(endTime - startTime.current);

      setReactionTime(time);
      setGameState("result");
    }

    if (gameState === "result" || gameState === "too-early") {
      startGame();
    }
  };

  useEffect(() => {
    return () => clearTimeout(timer.current);
  }, []);

  return (
    <main className={`game ${gameState}`} onClick={handleClick}>
      <div className="content">
        <h1>Reaction Time Test</h1>

        {gameState === "idle" && (
          <>
            <p>Click anywhere to start</p>
            <button>START</button>
          </>
        )}

        {gameState === "waiting" && (
          <>
            <h2>Wait...</h2>
            <p>Don't click yet!</p>
          </>
        )}

        {gameState === "ready" && (
          <>
            <h2>CLICK!</h2>
            <p>Click as fast as you can</p>
          </>
        )}

        {gameState === "too-early" && (
          <>
            <h2>Too Early!</h2>
            <p>Click to try again</p>
          </>
        )}

        {gameState === "result" && (
          <>
            <h2>{reactionTime} ms</h2>
            <p>Click to try again</p>
          </>
        )}
      </div>
    </main>
  );
}


3. app/globals.css

Replace it with:

* {
  box-sizing: border-box;
}

html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
}

body {
  font-family: Arial, sans-serif;
}

.game {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  cursor: pointer;
  transition: background 0.15s ease;
  user-select: none;
}

.game.idle {
  background: #222;
  color: white;
}

.game.waiting {
  background: #d9534f;
  color: white;
}

.game.ready {
  background: #28a745;
  color: white;
}

.game.too-early {
  background: #f0ad4e;
  color: white;
}

.game.result {
  background: #222;
  color: white;
}

.content {
  padding: 30px;
}

h1 {
  font-size: 52px;
  font-weight: 900;
  margin-bottom: 25px;
}

h2 {
  font-size: 80px;
  font-weight: 900;
  margin: 20px 0;
}

p {
  font-size: 20px;
}

button {
  padding: 22px 60px;
  font-size: 28px;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 1px;

  border: none;
  border-radius: 14px;

  background: white;
  color: #111;

  cursor: pointer;

  box-shadow: 0 8px 0 rgba(0, 0, 0, 0.25);

  transition: transform 0.1s ease, box-shadow 0.1s ease;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 0 rgba(0, 0, 0, 0.25);
}

button:active {
  transform: translateY(6px);
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.25);
}



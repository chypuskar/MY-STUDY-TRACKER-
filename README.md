// README.md

Study Progress Tracker 📚

A modern web app to track study progress with targets, ideal pacing, and real-time analytics.

Features

📊 Dashboard with Target vs Real Progress

📅 Ideal Progress vs Actual Progress (based on target days)

🧩 Chapter & Subchapter tracking

⏱️ Remaining days per chapter/subchapter

🎨 Clean, modern UI (Tailwind + charts)


Tech Stack

React (Vite)

TailwindCSS

Recharts

LocalStorage (can be replaced with backend)


Setup

npm install
npm run dev


---

// src/App.jsx import { useState } from "react"; import Dashboard from "./components/Dashboard";

export default function App() { const [course, setCourse] = useState({ title: "My Course", totalDays: 30, chapters: [ { name: "Chapter 1", subchapters: [ { name: "1.1", completed: false }, { name: "1.2", completed: false } ] } ] });

return ( <div className="min-h-screen bg-gray-100 p-6"> <h1 className="text-3xl font-bold mb-4">Study Tracker</h1> <Dashboard course={course} setCourse={setCourse} /> </div> ); }

// src/components/Dashboard.jsx import ProgressChart from "./ProgressChart";

export default function Dashboard({ course, setCourse }) { const totalSub = course.chapters.reduce( (acc, ch) => acc + ch.subchapters.length, 0 );

const completed = course.chapters.reduce( (acc, ch) => acc + ch.subchapters.filter((s) => s.completed).length, 0 );

const progress = (completed / totalSub) * 100;

const idealPerDay = totalSub / course.totalDays;

return ( <div className="grid grid-cols-1 lg:grid-cols-2 gap-6"> {/* LEFT COLUMN */} <div className="bg-white p-4 rounded-2xl shadow"> <h2 className="text-xl font-semibold mb-3">Targets</h2> {course.chapters.map((ch, i) => ( <div key={i} className="mb-3"> <h3 className="font-bold">{ch.name}</h3> {ch.subchapters.map((sub, j) => ( <label key={j} className="flex items-center gap-2"> <input type="checkbox" checked={sub.completed} onChange={() => { const updated = { ...course }; updated.chapters[i].subchapters[j].completed = !sub.completed; setCourse(updated); }} /> {sub.name} </label> ))} </div> ))} </div>

{/* RIGHT COLUMN */}
  <div className="bg-white p-4 rounded-2xl shadow">
    <h2 className="text-xl font-semibold mb-3">Progress</h2>
    <p>Completed: {completed}/{totalSub}</p>
    <p>Progress: {progress.toFixed(2)}%</p>
    <p>Ideal/day: {idealPerDay.toFixed(2)} subchapters</p>

    <ProgressChart progress={progress} ideal={idealPerDay} />
  </div>

  {/* IDEAL VS REAL */}
  <div className="col-span-2 bg-white p-4 rounded-2xl shadow">
    <h2 className="text-xl font-semibold mb-3">Ideal vs Real</h2>
    <ProgressChart progress={progress} ideal={idealPerDay * 100} />
  </div>

  {/* SUGGESTIONS */}
  <div className="col-span-2

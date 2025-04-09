# skilytix
import React from "react";
import { BrowserRouter as Router, Route, Routes, Link } from "react-router-dom";
import { useState } from "react";

import "./index.css"; // Tailwind CSS should be imported here

function App() {
  return (
    <Router>
      <div className="min-h-screen bg-pink-50 text-gray-800">
        <Navbar />
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/form" element={<SkinForm />} />
          <Route path="/results" element={<Results />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </div>
    </Router>
  );
}

function Navbar() {
  return (
    <nav className="bg-white shadow p-4 flex justify-between items-center">
      <h1 className="text-xl font-bold text-pink-600">Skinlytix</h1>
      <div className="space-x-4">
        <Link to="/" className="hover:text-pink-500">Home</Link>
        <Link to="/form" className="hover:text-pink-500">Analyze</Link>
        <Link to="/results" className="hover:text-pink-500">Results</Link>
        <Link to="/about" className="hover:text-pink-500">About</Link>
      </div>
    </nav>
  );
}

function Home() {
  return (
    <div className="text-center p-10">
      <h2 className="text-4xl font-bold text-pink-700 mb-4">Welcome to Skinlytix</h2>
      <p className="text-lg text-gray-700 mb-6 italic">Where beauty and science meet</p>
      <Link to="/form" className="bg-pink-500 text-white px-4 py-2 rounded-full hover:bg-pink-600">Start Your Analysis</Link>
    </div>
  );
}

function SkinForm() {
  const [formData, setFormData] = useState({
    skinType: "",
    concerns: [],
  });

  const skinConcerns = [
    "Acne", "Dryness", "Oiliness", "Pigmentation",
    "Redness", "Sensitivity", "Aging", "Dullness",
    "Dark Circles", "Uneven Skin Tone"
  ];

  const handleCheckboxChange = (concern) => {
    setFormData(prev => {
      const newConcerns = prev.concerns.includes(concern)
        ? prev.concerns.filter(c => c !== concern)
        : [...prev.concerns, concern];
      return { ...prev, concerns: newConcerns };
    });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    localStorage.setItem("skinData", JSON.stringify(formData));
    window.location.href = "/results";
  };

  return (
    <form onSubmit={handleSubmit} className="max-w-xl mx-auto p-6">
      <h2 className="text-2xl font-bold text-pink-600 mb-4">Skin Analysis</h2>

      <label className="block mb-2">Skin Type</label>
      <select className="w-full mb-4 p-2 border rounded" onChange={e => setFormData({ ...formData, skinType: e.target.value })}>
        <option value="">Select your skin type</option>
        <option value="Normal">Normal</option>
        <option value="Oily">Oily</option>
        <option value="Dry">Dry</option>
        <option value="Combination">Combination</option>
        <option value="Sensitive">Sensitive</option>
      </select>

      <label className="block mb-2">Skin Concerns</label>
      <div className="grid grid-cols-2 gap-2 mb-4">
        {skinConcerns.map((concern) => (
          <label key={concern} className="flex items-center space-x-2">
            <input
              type="checkbox"
              checked={formData.concerns.includes(concern)}
              onChange={() => handleCheckboxChange(concern)}
            />
            <span>{concern}</span>
          </label>
        ))}
      </div>

      <button className="bg-mint-500 hover:bg-mint-600 text-white px-4 py-2 rounded-full" type="submit">
        Get Recommendations
      </button>
    </form>
  );
}

function Results() {
  const data = JSON.parse(localStorage.getItem("skinData")) || {};

  return (
    <div className="p-6 max-w-xl mx-auto">
      <h2 className="text-2xl font-bold text-pink-600 mb-4">Your Recommendations</h2>
      <p className="mb-2">Skin Type: <strong>{data.skinType}</strong></p>
      <p className="mb-4">Concerns: {data.concerns?.join(", ")}</p>
      <p>✨ Based on your analysis, we recommend products with gentle ingredients, hydration boosters, and targeted actives. A full AI-driven breakdown is coming soon!</p>
    </div>
  );
}

function About() {
  return (
    <div className="p-6 max-w-3xl mx-auto text-center">
      <h2 className="text-2xl font-bold text-pink-600 mb-4">About Skinlytix</h2>
      <p>
        Skinlytix is a modern skincare platform built on the belief that beauty and science can work together.
        Our mission is to help people discover the right skincare products through intelligent analysis of their skin.
        No fluff, just results—tailored for your unique skin.
      </p>
    </div>
  );
}

export default App;

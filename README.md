# Vending-machine
import React, { useState, useEffect } from "react";

export default function VendingMachineApp() {
  const [time, setTime] = useState(new Date());
  const [selected, setSelected] = useState([]);
  const [result, setResult] = useState([]);

  const snacks = {
    Drinks: ["Mountain Dew"],
    Candy: ["Gummy Bears"],
    Chips: ["Doritos", "Pringles"],
  };

  useEffect(() => {
    const timer = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(timer);
  }, []);

  const toggleCategory = (category) => {
    setSelected((prev) =>
      prev.includes(category)
        ? prev.filter((c) => c !== category)
        : [...prev, category]
    );
  };

  const generateRecommendations = () => {
    const recommendations = selected.flatMap((c) => snacks[c]);
    setResult(recommendations);
  };

  return (
    <div className="min-h-screen bg-gray-100 flex flex-col items-center p-6">
      {/* Digital Clock */}
      <div className="text-4xl font-bold mb-6">
        {time.toLocaleTimeString()}
      </div>

      {/* Vending Machine */}
      <div className="bg-black text-white rounded-2xl p-6 w-80 shadow-lg mb-8">
        <h1 className="text-2xl font-bold mb-4">Vending Machine</h1>
        <div className="bg-white text-black rounded-xl p-4 mb-4">
          <p>🥤 Mountain Dew</p>
          <p>🍬 Gummy Bears</p>
        </div>
        <div className="flex justify-around text-2xl">
          <span>💳 Card</span>
          <span>💵 Cash</span>
        </div>
      </div>

      {/* Snack Selector */}
      <div className="bg-white rounded-2xl p-6 w-full max-w-md shadow-lg">
        <h2 className="text-2xl font-bold mb-4">Snack Selector</h2>
        <div className="grid grid-cols-2 gap-4 mb-6">
          {Object.keys(snacks).map((category) => (
            <button
              key={category}
              onClick={() => toggleCategory(category)}
              className={`p-4 rounded-xl text-lg font-bold transition-all shadow ${
                selected.includes(category)
                  ? "bg-pink-500 text-white scale-105"
                  : "bg-yellow-100 hover:bg-yellow-200"
              }`}
            >
              {category}
            </button>
          ))}
        </div>

        <button
          onClick={generateRecommendations}
          className="w-full bg-green-500 text-white text-xl font-bold py-3 rounded-xl hover:scale-105 transition shadow-lg"
        >
          Submit & Generate Recommendations
        </button>

        {result.length > 0 && (
          <div className="mt-6 bg-purple-50 rounded-xl p-4">
            <h3 className="text-xl font-bold mb-3">Recommended Snacks</h3>
            {result.map((item, index) => (
              <div
                key={index}
                className="bg-white rounded-lg p-3 shadow flex items-center justify-between mb-2"
              >
                <span className="font-semibold">{item}</span>
                <span className="text-2xl">
                  {item.includes("Mountain")
                    ? "🥤"
                    : item.includes("Gummy")
                    ? "🍬"
                    : "🍿"}
                </span>
              </div>
            ))}
          </div>
        )}
      </div>
    </div>
  );
}

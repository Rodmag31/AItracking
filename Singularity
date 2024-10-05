import React, { useState, useMemo } from 'react';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts';

// Constants and initial data
const scenarios = {
  baseline: 'Baseline',
  rapidAdvancement: 'Rapid Technological Advancements',
  regulatoryBarriers: 'Regulatory Barriers',
  technologicalDecline: 'Technological Decline',
  average: 'Average Scenario'
};

const initialData = {
  baseline: [
    { year: 2024, A: 5, B: 15, C: 25 },
    { year: 2030, A: 20, B: 30, C: 20 },
    { year: 2040, A: 60, B: 35, C: 10 },
    { year: 2050, A: 90, B: 20, C: 5 },
    { year: 2060, A: 100, B: 10, C: 0 }
  ],
  rapidAdvancement: [
    { year: 2024, A: 10, B: 20, C: 30 },
    { year: 2030, A: 40, B: 35, C: 15 },
    { year: 2040, A: 80, B: 25, C: 5 },
    { year: 2050, A: 100, B: 10, C: 0 },
    { year: 2060, A: 100, B: 5, C: 0 }
  ],
  regulatoryBarriers: [
    { year: 2024, A: 3, B: 12, C: 22 },
    { year: 2030, A: 10, B: 25, C: 25 },
    { year: 2040, A: 30, B: 40, C: 20 },
    { year: 2050, A: 60, B: 30, C: 15 },
    { year: 2060, A: 85, B: 20, C: 5 }
  ],
  technologicalDecline: [
    { year: 2024, A: 5, B: 15, C: 25 },
    { year: 2030, A: 10, B: 20, C: 30 },
    { year: 2040, A: 15, B: 25, C: 35 },
    { year: 2050, A: 20, B: 30, C: 40 },
    { year: 2060, A: 25, B: 35, C: 45 }
  ]
};

const contextDescriptions = {
  2024: "Early stages of AI development, with dependent AI systems dominating.",
  2030: "Independent AI systems gain traction, semi-dependent systems peak.",
  2040: "Rapid growth in independent AI, decline in dependent systems.",
  2050: "Independent AI approaches singularity, other forms become obsolete.",
  2060: "Potential achievement of AI singularity, complete dominance of independent AI."
};

const aiTypes = {
  A: "Independent AI: Systems with autonomous evolution towards ASI.",
  B: "Semi-dependent AI: Hybrid systems merging human and machine capabilities.",
  C: "Dependent AI: Systems relying on human governance and supervision."
};

const scenarioDescriptions = {
  baseline: "AI development follows a steady path with expected growth rates.",
  rapidAdvancement: "AI is advancing at an unprecedented pace due to breakthroughs in quantum computing and neural network architectures.",
  regulatoryBarriers: "Regulations are slowing the growth of independent AI, giving more time for semi-dependent systems to mature.",
  technologicalDecline: "Global crises and resource scarcity have led to a slowdown in AI development, favoring more conservative approaches.",
  average: "This scenario represents the average of all other scenarios, providing a balanced view of potential AI evolution paths."
};

// Custom hooks
const useScenarioData = (initialData, scenario) => {
  const calculateAverageData = (data) => {
    const years = data.baseline.map(item => item.year);
    return years.map((year, index) => {
      const A = (data.baseline[index].A + data.rapidAdvancement[index].A + data.regulatoryBarriers[index].A + data.technologicalDecline[index].A) / 4;
      const B = (data.baseline[index].B + data.rapidAdvancement[index].B + data.regulatoryBarriers[index].B + data.technologicalDecline[index].B) / 4;
      const C = (data.baseline[index].C + data.rapidAdvancement[index].C + data.regulatoryBarriers[index].C + data.technologicalDecline[index].C) / 4;
      return { year, A: Math.round(A), B: Math.round(B), C: Math.round(C) };
    });
  };

  const data = useMemo(() => {
    return scenario === 'average' ? calculateAverageData(initialData) : initialData[scenario];
  }, [initialData, scenario]);

  return data;
};

// Components
const CustomTooltip = ({ active, payload, label, scenario }) => {
  if (active && payload && payload.length) {
    return (
      <div className="bg-white p-3 border rounded shadow-lg">
        <h3 className="font-bold text-lg mb-2">{`Year: ${label}`}</h3>
        {payload.map((entry) => (
          <p key={entry.name} className="text-sm" style={{ color: entry.color }}>
            {`${entry.name}: ${entry.value}`}
          </p>
        ))}
        <p className="text-sm mt-2 text-gray-700">{contextDescriptions[label]}</p>
        <p className="text-xs italic mt-2 text-gray-600">{scenarioDescriptions[scenario]}</p>
      </div>
    );
  }
  return null;
};

const AIEvolutionTracker = () => {
  const [scenario, setScenario] = useState('baseline');
  const [adjustedData, setAdjustedData] = useState([...initialData.baseline].map(item => ({ ...item })));
  const originalData = useScenarioData(initialData, scenario);

  const handleScenarioChange = (e) => {
    const newScenario = e.target.value;
    setScenario(newScenario);
    setAdjustedData([...initialData[newScenario]].map(item => ({ ...item })));
  };

  const handleDataChange = (index, field, value) => {
    const newValue = Math.min(Math.max(Number(value), 0), 100);
    const newData = [...adjustedData];
    newData[index] = { ...newData[index], [field]: newValue };
    setAdjustedData(newData);
  };

  const handleReset = () => {
    const resetData = [...initialData[scenario]].map(item => ({ ...item }));
    setAdjustedData(resetData);
  };

  return (
    <div className="p-4">
      <h2 className="text-3xl font-bold text-center text-gray-800 mb-6">AI Evolution towards Singularity</h2>
      
      <div className="flex justify-center items-center mb-6">
        <label className="mr-2 text-gray-700">Scenario:</label>
        <select 
          value={scenario} 
          onChange={handleScenarioChange}
          className="border rounded px-3 py-2 bg-white text-gray-800 focus:outline-none focus:ring-2 focus:ring-blue-500"
        >
          {Object.entries(scenarios).map(([key, value]) => (
            <option key={key} value={key}>{value}</option>
          ))}
        </select>
      </div>

      <ResponsiveContainer width="100%" height={400}>
        <LineChart data={adjustedData}>
          <CartesianGrid strokeDasharray="3 3" />
          <XAxis dataKey="year" />
          <YAxis />
          <Tooltip content={<CustomTooltip scenario={scenario} />} />
          <Legend />
          <Line type="monotone" dataKey="A" stroke="#000080" name="Independent AI" />
          <Line type="monotone" dataKey="B" stroke="#008000" name="Semi-dependent AI" />
          <Line type="monotone" dataKey="C" stroke="#FFA500" name="Dependent AI" />
        </LineChart>
      </ResponsiveContainer>

      <div className="mt-8 bg-white p-6 rounded shadow-lg">
        <div className="flex justify-between items-center mb-4">
          <h3 className="text-2xl font-semibold text-gray-800">Adjust Data</h3>
          <button
            onClick={handleReset}
            className="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          >
            Reset Adjusted Values
          </button>
        </div>
        <div className="overflow-x-auto">
          <table className="w-full">
            <thead className="bg-gray-200">
              <tr>
                <th className="px-4 py-2">Year</th>
                <th className="px-4 py-2">A (Independent)</th>
                <th className="px-4 py-2">B (Semi-dependent)</th>
                <th className="px-4 py-2">C (Dependent)</th>
              </tr>
            </thead>
            <tbody>
              {adjustedData.map((item, index) => (
                <tr key={item.year} className="hover:bg-gray-100">
                  <td className="border px-4 py-2">{item.year}</td>
                  <td className="border px-4 py-2">
                    <input
                      type="number"
                      value={item.A}
                      onChange={(e) => handleDataChange(index, 'A', e.target.value)}
                      className="w-full border rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-blue-500"
                    />
                  </td>
                  <td className="border px-4 py-2">
                    <input
                      type="number"
                      value={item.B}
                      onChange={(e) => handleDataChange(index, 'B', e.target.value)}
                      className="w-full border rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-blue-500"
                    />
                  </td>
                  <td className="border px-4 py-2">
                    <input
                      type="number"
                      value={item.C}
                      onChange={(e) => handleDataChange(index, 'C', e.target.value)}
                      className="w-full border rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-blue-500"
                    />
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </div>

      <div className="mt-8 bg-white p-6 rounded shadow-lg">
        <h3 className="text-2xl font-semibold mb-4 text-gray-800">AI Types</h3>
        {Object.entries(aiTypes).map(([key, value]) => (
          <p key={key} className="mb-2"><strong>{key}:</strong> {value}</p>
        ))}
      </div>
    </div>
  );
};

export default AIEvolutionTracker;

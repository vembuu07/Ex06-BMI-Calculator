# Ex06 BMI Calculator
## Date:16.11.2025

## AIM
To create a BMI calculator using React Router 

## ALGORITHM
### STEP 1 State Initialization
Manage the current page (Home or Calculator) using React Router.

### STEP 2 User Input
Accept weight and height inputs from the user.

### STEP 3 BMI Calculation
Calculate the BMI based on user input.

### STEP 4 Categorization
Classify the BMI result into categories (Underweight, Normal weight, Overweight, Obesity).

### STEP 5 Navigation
Navigate between pages using React Router.

## PROGRAM

index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BMI Calculator</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
```

App.js

```
import React from "react";
import BMICalculator from "./BMICalculator";

function App() {
  return (
    <div className="app">
      <BMICalculator />
    </div>
  );
}

export default App;
```

BMICalculator.js

```
import React, { useState, useMemo } from "react";

export default function BMICalculator() {
  const [units, setUnits] = useState("metric");
  const [weight, setWeight] = useState(70);
  const [height, setHeight] = useState(170);
  const [error, setError] = useState("");

  const validateInputs = (w, h) => {
    if (w <= 0 || h <= 0) return "Values must be greater than zero.";
    if (w > 1000 || h > 3000) return "Values look unrealistic.";
    return "";
  };

  const validationError = useMemo(() => validateInputs(weight, height), [weight, height]);

  const calculateBMI = () => {
    if (validationError) return null;
    if (units === "metric") return weight / ((height / 100) ** 2);
    return (703 * weight) / (height ** 2);
  };

  const bmi = useMemo(() => {
    const val = calculateBMI();
    return val == null ? null : Number(val.toFixed(2));
  }, [units, weight, height, validationError]);

  const bmiCategory = (val) => {
    if (val == null) return null;
    if (val < 18.5) return "Underweight";
    if (val < 25) return "Normal";
    if (val < 30) return "Overweight";
    return "Obese";
  };

  const category = bmiCategory(bmi);

  const reset = () => {
    setUnits("metric");
    setWeight(70);
    setHeight(170);
    setError("");
  };

  return (
    <div className="calculator-container">
      <h1>BMI Calculator</h1>

      <div className="unit-toggle">
        <button className={units === "metric" ? "active" : ""} onClick={() => setUnits("metric")}>Metric (kg, cm)</button>
        <button className={units === "imperial" ? "active" : ""} onClick={() => setUnits("imperial")}>Imperial (lb, in)</button>
      </div>

      <form onSubmit={(e) => e.preventDefault()}>
        <label>Weight ({units === "metric" ? "kg" : "lb"})</label>
        <input type="number" value={weight} onChange={(e) => setWeight(Number(e.target.value))} />

        <label>Height ({units === "metric" ? "cm" : "in"})</label>
        <input type="number" value={height} onChange={(e) => setHeight(Number(e.target.value))} />

        {validationError && <p className="error">{validationError}</p>}

        <div className="buttons">
          <button type="button" onClick={reset}>Reset</button>
        </div>
      </form>

      <div className="result">
        {bmi != null && (
          <>
            <p>BMI: {bmi}</p>
            <p>Category: {category}</p>
          </>
        )}
      </div>
    </div>
  );
}

```

App.css

```
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  margin: 0;
  padding: 0;
}

.app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.calculator-container {
  background: #fff;
  padding: 20px 30px;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  width: 300px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

.unit-toggle {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
}

.unit-toggle button {
  flex: 1;
  margin: 0 2px;
  padding: 8px;
  border: 1px solid #ccc;
  background: #eee;
  cursor: pointer;
}

.unit-toggle button.active {
  background: #4f46e5;
  color: white;
  border-color: #4f46e5;
}

label {
  display: block;
  margin-top: 10px;
}

input {
  width: 100%;
  padding: 6px;
  margin-top: 4px;
  margin-bottom: 10px;
  border-radius: 6px;
  border: 1px solid #ccc;
}

.buttons {
  display: flex;
  justify-content: space-between;
}

.buttons button {
  padding: 6px 12px;
  cursor: pointer;
}

.error {
  color: red;
  font-size: 0.9em;
}

.result {
  margin-top: 15px;
  text-align: center;
  font-weight: bold;
}

```

index.js

```
import React from "react";
import { createRoot } from "react-dom/client";
import App from "./App";
import "./App.css";

const root = createRoot(document.getElementById("root"));
root.render(<App />);
```

package.json

```
{
  "name": "cac",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/dom": "^10.4.1",
    "@testing-library/jest-dom": "^6.8.0",
    "@testing-library/react": "^16.3.0",
    "@testing-library/user-event": "^13.5.0",
    "react": "^19.1.1",
    "react-dom": "^19.1.1",
    "react-scripts": "5.0.1",
    "web-vitals": "^2.1.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}

```
## OUTPUT

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/86ade3b4-6473-4b66-8c14-497d46053bda" />

## RESULT
The program for creating BMI Calculator using React Router is executed successfully.

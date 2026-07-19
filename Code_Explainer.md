# Code Explainer: Clinical Trial Safety Analytics Pipeline
*This document breaks down the Python code used in the patient safety analysis notebook line-by-line for non-technical stakeholders.*

---

## 1. The Simulation Engine (How We Built the Data)
To analyze a clinical trial safely without using real, private patient medical records, we used Python's numerical libraries to build a realistic data sandbox.

* **`np.random.seed(2026)`**: This locks down the computer's random number generator. It ensures that every single time someone runs this code, it generates the exact same numbers, making our project fully scientific and reproducible.
* **`np.random.normal(loc=26.5, scale=5.0)`**: This generates realistic patient Body Mass Indexes (BMIs). Instead of completely random numbers, it forces the data to follow a natural human "bell curve" centered around a typical average BMI of 26.5.

### The Hidden Rule (The "Ground Truth")
* **`toxicity_score = (0.005 * drug_dosage_mg) + (0.12 * bmi) ...`**: This line is where we programmed a hidden medical rule into the data. We purposefully made BMI hold a much higher mathematical weight (0.12) than the drug dosage size (0.005), meaning a patient's body composition impacts their risk far more violently than the drug size alone.
* **`1 / (1 + np.exp(-toxicity_score))`**: This is the **Sigmoid Function**. It takes our raw toxicity scores and compresses them into a clean percentage scale between 0% and 100%, giving us a precise mathematical probability of a patient having a bad medical reaction.

---

## 2. The Data Cleaning Step (Handling the Imperfections)
Real business data is never perfect. We intentionally programmed the data engine to drop biometric data for older patients to simulate real-world logging errors.

* **`df.isnull().sum()`**: This scanning command searches our entire dataset and counts up the blanks. It revealed that exactly 129 elderly patients were completely missing their BMI information.
* **`df['BMI'].fillna(median_bmi)`**: If we simply deleted those 129 people, our study would completely lose track of older patients, making our drug safety models highly inaccurate for the elderly. This line automatically finds every blank space and fills it with the middle-ground BMI value of the healthy population, keeping our dataset perfectly balanced without losing valuable records.

---

## 3. The Machine Learning Model (Deciphering the Rules)
Once our data was perfectly clean, we ran a machine learning algorithm called **Logistic Regression** to see if the computer could uncover the hidden medical rules we programmed in Step 1 *without* looking at the original instructions.

* **`LogisticRegression().fit(X, y)`**: This line feeds our patient metrics (Age, BMI, Dosage) into the AI model and commands it to find the patterns. It looks at who had an adverse reaction and who didn't, working backward to calculate the exact risk multipliers.
* **`model.coef_`**: These are the final coefficients (the weights). When our model output **`BMI: 0.1239`**, it proved the AI successfully deciphered our data sandbox perfectly from scratch, giving us the hard mathematical proof we used to write our executive safety guidelines.

# Engine RPM ↔ Speed Calculator

Live demo: [https://ganesh-solapure.github.io/](https://ganesh-solapure.github.io/)

A lightweight client-side tool to convert between engine RPM, wheel RPM, vehicle speed, transmission ratio, and tire diameter—all within your browser, no backend required.

---

##  Features

- **Parse tyre code** like `185/55R15` to automatically compute tire diameter (in inches).
- Convert between **Vehicle Speed ↔ Wheel RPM ↔ Engine RPM** using provided formulas.
- **Adjust units**: support for both mph and km/h.
- Save and load **presets** (e.g., for specific cars or gear ratios) using `localStorage`.
- View a **RPM vs Speed** plot for better visualization.
- Connect to a **secure WebSocket (WSS)** placeholder for live data integration (e.g., from an embedded device).
- Optimized for **low CPU usage**: chart updates are efficient, redraws are minimized, and WebSocket handling is throttled.

---

##  How to Use

1. Navigate to: `https://ganesh-solapure.github.io/`.
2. Input:
   - **Vehicle Speed** and choose unit (mph/kmh).
   - **Tire Diameter** (in inches) **or** enter a tyre code like `185/55R15` then click **Parse**.
   - **Transmission Ratio** (e.g., your gear ratio).
3. Click **Calculate** to get:
   - **Wheel RPM**
   - **Engine RPM**
4. Explore the **RPM vs Speed** chart for insight into operating ranges.
5. Use **Save Preset** to store combinations of inputs; retrieve them from the dropdown.
6. (Optional) Provide a `wss://` URL to connect to live data — displayed in the panel if enabled.

---

##  Key Formulas

### 1. Tire Diameter from Tyre Code (e.g. `185/55R15`)
- **Tire width (W):** 185 mm  
- **Aspect ratio (A):** 55% → sidewall height = \( W \times \frac{A}{100} = 185 \times 0.55 = 101.75 \) mm  
- Convert mm to inches: \( \frac{101.75}{25.4} ≈ 4.01 \) in  
- **Rim diameter (R):** 15 in  
- **Tire diameter:** \( D = R + 2 \times \text{(sidewall\_inches)} ≈ 15 + 2 \times 4.01 ≈ 23 \text{in} \)

### 2. Wheel RPM from Vehicle Speed
\[
\text{Wheel RPM} = \frac{\text{Speed (mph)} \times 1056}{\pi \times D}
\]
- 1056 comes from converting miles per hour to inches per minute (63360 in/mile ÷ 60 min/hour).
- \( D \) is tire diameter in inches.

### 3. Engine RPM from Wheel RPM
\[
\text{Engine RPM} = \text{Wheel RPM} \times \text{Transmission Ratio}
\]

### 4. Optional — Transmission Ratio from Known RPMs
\[
\text{Transmission Ratio} = \frac{\text{Engine RPM}}{\text{Wheel RPM}}
\]

---

##  Code Structure Overview

- **Chart Initialization**: Created once using [Chart.js](https://www.chartjs.org/) with animations disabled to minimize redraw overhead.
- **Efficient Updates**: Both calculations and chart updates are throttled and optimized to prevent browser lag.
- **WebSocket Integration**: WebSocket messages are parsed and UI updates are limited to ~5 updates per second for stable performance.
- **Local Presets**: Stored in `localStorage` under the key `rpm_calc_presets_v1`—easy to manage and persistent.
- **Responsive Design**: Adapts layout for both desktop and mobile, while maintaining low resource usage.

---

##  Development & Running Locally

1. Clone the repo:
    ```bash
    git clone https://github.com/ganesh-solapure/ganesh-solapure.github.io.git
    cd ganesh-solapure.github.io
    ```
2. Launch a simple local server:
   ```bash
   python -m http.server 8000

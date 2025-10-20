Temperature Application Development – Report
1) Objective
Design and implement a browser-based VI-style application that:
Converts an input temperature from °C to °F.
Compares the computed °F to a user-defined set temperature (°F).
Indicates system status using two LEDs (Red/Green) and one message.
Supports continuous monitoring via a RUN loop and a SAFETY STOP.
2) Problem Statement (given)
If the computed °F value is greater than the set temperature, the RED LED turns on and the message “Please turn OFF the system” is displayed. Otherwise, the GREEN LED turns on with the message “System is safe.” Only one LED and one message may be active at a time.
3) Inputs
Temperature (°C): vertical slider with digital display, range 0–100 °C.
Set temperature (°F): numeric control (integer step), user-editable threshold.
4) Formula (Formula Node equivalent)
 
∘F=∘C×1.8+32∘F=∘C×1.8+32 
​	
 
This conversion is recomputed whenever inputs change or while the loop is running.
5) Process / Algorithm (case + local variables)
Read °C from the vertical slider.
Compute °F using 
∘F=∘ ⁣C×1.8+32∘F= ∘C×1.8+32.
Compare: if °F > setpoint → Over-temperature case; else → Safe case.
Update indicators (mutually exclusive):
Over-temperature: RED LED ON, GREEN LED OFF, message “Please turn OFF the system.”
Safe: GREEN LED ON, RED LED OFF, message “System is safe.”
Loop control:
RUN: starts a timed loop (~200 ms period) that repeats steps 1–4 (equivalent to a while loop in a VI).
SAFETY STOP: stops the loop, turns LEDs off, and shows “Stopped.”
Local variables hold the current °C, °F, setpoint, and LED/message states across loop iterations.
6) Outputs (UI elements)
Digital displays:
Current °C (from slider).
Computed °F (one decimal).
Setpoint °F (integer).
Thermometer indicator for visual °F feedback (clamped to 32–212 °F for realism).
LEDs: one RED, one GREEN (only one is ON).
Single message display:
Over-temp: “Please turn OFF the system”
Safe: “System is safe”
7) Testing Procedure
Mode: Click RUN (do not use continuous button) to start the loop.
Safe state test
Setpoint: 140 °F
Slider: 25 °C → 25×1.8+32=77°F
25×1.8+32=77°F
Expected: GREEN LED ON, message “System is safe.”
Over-temperature test
Keep setpoint 140 °F
Slider: 90 °C → 90×1.8+32=194°F
90×1.8+32=194°F
Expected: RED LED ON, message “Please turn OFF the system.”
Boundary test (strict “greater than”)
Choose °C so that °F = setpoint exactly.
Expected: GREEN LED ON (safe), since condition is °F > setpoint, not ≥.
Loop control test
Press RUN, move the slider and observe real-time updates.
Press SAFETY STOP and verify the loop stops and LEDs turn off.
8) Results / Observations
The application updates °F, LEDs, and message in real time during the RUN loop.
The strict “>” comparison correctly keeps the system safe at the boundary.
The UI enforces one message and mutually exclusive LEDs at all times.
Thermometer visualization provides intuitive feedback without affecting logic.
9) User Experience & Styling
Clear labels, large digits, and distinct colors (red/green) aid quick status checks.
RUN and SAFETY STOP buttons mimic VI semantics (while loop start/stop).
Works entirely in the browser; easy to deploy to GitHub Pages or Netlify.
10) Conclusion
The app satisfies the assignment requirements:
Correct °C→°F conversion using the specified formula.
Proper decision logic to drive two LEDs and a single message.
Continuous operation via a while-like loop with a safety stop.
Clean, user-friendly interface with appropriate indicators.

# Closed-loop-control-of-pathological-neural-oscillations-using-Simulink
The objective of this mini-project is to develop a conceptual closed-loop control framework in MATLAB/Simulink for studying the suppression of pathological neural oscillations. The neural dynamics are represented by the FitzHugh–Nagumo model, and a feedback pathway is proposed to detect oscillatory activity and adjust the model's stimulation input.

The specific objectives are:

1. To represent excitable neuronal dynamics using the nonlinear FitzHugh–Nagumo differential equations.
2. To generate and analyze a simulated neural signal, including the effect of an added noise term, was implemented.
3. To propose digital bandpass filtering in the 4–8 Hz band and RMS-based feature extraction as a scalar measure of oscillatory activity.
4. To compare the extracted feature with a reference and use a PID or state-feedback controller to determine a corrective control signal.
5. To model a stimulation pulse generator and feed its output back to the neural plant.
6. To define a simulation procedure for assessing whether feedback reduces the selected oscillation measure, without assuming a successful outcome before simulation.

2. Brief Introduction:
   
Neural systems are nonlinear dynamical systems capable of generating electrical activity over a range of temporal scales. In some neurological conditions, neural populations can exhibit
abnormal or excessively synchronized oscillatory activity. Parkinson’s disease is associated with motor symptoms and altered rhythmic activity in basal ganglia circuits. Deep brain stimulation (DBS), which delivers electrical stimulation through implanted electrodes, is an established treatment for selected patients with movement disorders.
Conventional DBS is commonly delivered continuously using parameters selected during clinical programming. A closed-loop or adaptive approach instead uses a measured physiological signal to adjust stimulation according to the observed state of the system. In principle, feedback can avoid delivering the same stimulation level at all times. The practical design must, however, account for signal noise, delays, actuator limits, stability, and the relationship between the measured biomarker and the desired clinical effect.

This project studies the control concept in simulation rather than in a biological preparation or medical device. The FitzHugh–Nagumo model is selected because it captures essential excitable behavior with two coupled nonlinear state variables: a fast, membrane-potential-like variable and a slower recovery variable. It is simpler to simulate than detailed ionic-channel models, making it suitable for an introductory systems-simulation study.
The proposed signal-processing chain isolates activity in a selected 4–8 Hz band and estimates its short-term magnitude using a moving root-mean-square (RMS) operation. The resulting feature is compared with a reference, and a controller is intended to modify a stimulation input to the neural model. These choices define the proposed architecture; their performance remains to be established through model runs.

3. Literature Review:
   
3.1 FitzHugh–Nagumo model
FitzHugh introduced a reduced nonlinear model of nerve membrane behavior that captures excitability and recovery using a small number of state variables [1]. Nagumo, Arimoto, and Yoshizawa subsequently presented an electrical analogue of nerve-axon dynamics [2]. The reduced model does not reproduce every ionic mechanism of a real neuron, but it can represent qualitative transitions between resting and excited behaviour. This trade-off makes it useful when the main aim is to investigate system-level dynamics and feedback concepts rather than detailed biophysics.

3.2 Deep brain stimulation and adaptive feedback
DBS provides electrical stimulation to targeted neural structures and can improve symptoms in selected movement-disorder patients. A limitation of a purely continuous strategy is that stimulation is not necessarily adjusted in response to moment-to-moment changes in neural activity. Little et al. demonstrated a proof-of-principle adaptive DBS approach in people with advanced Parkinson’s disease, using pathological brain activity to help control when stimulation was delivered [3]. This work supports the general feasibility of biomarker-guided stimulation, but does not imply that the simplified controller proposed here has been clinically validated.

3.3 Signal processing for biomarker extraction
A measured neural signal may contain slow drift, broadband noise, and components unrelated to the oscillation of interest. Digital bandpass filtering can focus analysis on a selected frequency interval. A moving RMS measure can then provide a time-varying estimate of the magnitude of the filtered waveform. In this proposal, the 4–8 Hz interval is selected to represent a tremor-related band described in the project synopsis. This is a modelling choice, not a universal biomarker for every patient or every Parkinsonian symptom.

3.4 Control systems perspective
A feedback controller compares a measured output with a reference and uses the error to determine the control input. A PID controller combines proportional, integral, and derivative actions. In a stimulation model, output saturation and anti-windup are relevant because the actuator cannot deliver arbitrarily large current. Controller performance depends on model parameters, gains, signal-processing delay, sampling rate, and the sign and strength of the stimulation-to-neural-state relationship. These must be checked in simulation rather than assumed.

4. Problem Gap
   
The literature establishes both simplified excitable-neuron models and the broader concept of adaptive DBS. For a systems-simulation laboratory project, an educational gap remains between these ideas: the neural plant, noisy signal measurement, frequency-selective feature extraction, controller, and stimulation actuator need to be represented together as a single feedback system that can be inspected and tested in Simulink.

The proposed project addresses this modeling gap by connecting a FitzHugh–Nagumo neural plant to a 4–8 Hz bandpass filter, an RMS feature estimator, an error detector, a bounded controller, and a stimulation pulse generator. The project will investigate whether the selected feature can be reduced under the chosen simulation conditions and whether the feedback loop behaves stably. Since implementation and testing have not yet been completed, reduction of oscillations is a target to be evaluated, not a result already demonstrated.

The model is deliberately simplified. It does not represent an anatomically detailed basal-ganglia network, patient-specific physiology, clinical sensing hardware, electrode-tissue interactions, or safety-certified stimulation. Therefore, the findings, once obtained, will apply to the simulated system and should not be interpreted as evidence of clinical efficacy.

5. Specifications of Proposal
   
5.1 Functional architecture
The proposed architecture contains four main subsystems: (i) a biological neural plant, (ii) signal conditioning and biomarker extraction, (iii) error detection and feedback control, and (iv) a stimulation actuator. The neural plant's output is measured, processed, and fed to the controller. The actuator output is then fed back into the neural plant as an external stimulation input.

FitzHugh–Nagumo neural plant → 4–8 Hz bandpass filter → moving RMS feature y(t) → error detector [r(t) − y(t)] → PID / state-feedback controller → pulse generator → stimulation input to neural plant

5.2 Neural plant model
This specifies the following FitzHugh–Nagumo form:

                                            dv/dt = v − v³/3 − w + I_ext + I_stim + η(t)
                                            dw/dt = (1/τ)(v + a − bw)

Here, v(t) is the fast state associated with membrane potential, w(t) is a slower recovery state, I_ext is the external bias current, I_stim is the control/stimulation input, and η(t) represents an optional noise input. The parameters a, b, and τ influence the model dynamics. Parameter values and initial conditions must be specified before simulation; they are not supplied in the synopsis and are therefore left for implementation.

5.3 Signal conditioning and feature extraction

Input: the simulated neural output v(t), sampled at a rate suitable for analyzing the selected frequency band.
Bandpass filter: a fourth-order discrete Butterworth bandpass filter with a proposed passband of 4–8 Hz.

Feature estimator: moving RMS of the filtered signal over a selected sliding window, yielding a non-negative feature y(t) that tracks signal magnitude.
For a window containing N samples, the RMS estimate can be written as y[k] = sqrt((1/N) Σ x_f²[i]), where x_f is the bandpass-filtered signal and the sum covers the selected window. The sampling frequency, filter implementation, and RMS window length must be documented when chosen.

5.4 Error detector and controller
The proposed reference is r(t) = 0, representing a target of minimal measured oscillatory power in the selected band. The synopsis defines the error as e(t) = r(t) − y(t). A PID controller is proposed, with proportional action responding to current error, integral action accumulating error, and derivative action responding to changes in error. Output saturation and anti-windup are proposed to limit excessive controller demand.
A design detail must be checked during implementation: since y(t) is non-negative and r(t) = 0, the defined error is non-positive whenever oscillatory activity is present. The controller sign, actuator mapping, and neural plant response must therefore be configured so that the resulting stimulation input acts in the intended direction. A PID controller does not automatically suppress oscillations simply because it is connected in a loop.

5.5 Stimulation actuator
The synopsis proposes a pulse generator producing a charge-balanced biphasic stimulation waveform with a nominal repetition rate of 130 Hz. The controller output is intended to modulate pulse amplitude or another defined stimulation parameter. The pulse waveform is then supplied as I_stim(t) to the neural plant. The pulse amplitude limits, pulse widths, phase balance, scaling, and control-to-amplitude mapping remain implementation parameters and must be specified before results can be interpreted.

5.6 Proposed specifications summary

Element                   | Proposed specification
Neural model              | FitzHugh–Nagumo nonlinear differential equations
Measured output           | Simulated fast state v(t)
Target feature band       | 4–8 Hz
Filter                    | Fourth-order discrete Butterworth bandpass (proposed)
Feature                   | Moving RMS of filtered output
Reference                 | r(t) = 0 for the selected oscillation feature
Controller                | PID with output limits and anti-windup, or state feedback if implemented
Stimulation               | Charge-balanced biphasic pulse train; nominal 130 Hz
Simulation environment    | MATLAB/Simulink

6. Methodology:
   
The following methodology is proposed for implementation in MATLAB/Simulink. It describes planned work and is not a claim that the simulations have already been performed.

Step 1: Implement the neural plant. Build the two coupled differential equations using Integrator blocks or an equivalent subsystem. Set initial conditions and select model parameters that produce the intended excitable or oscillatory behavior. Record the chosen solver and simulation step settings.

Step 2: Establish a baseline. Run the neural plant without feedback stimulation and inspect v(t) and w(t). Confirm that the baseline output is numerically stable and that the signal has the behavior required for the intended test. Do not label it pathological unless the chosen model settings and signal analysis support that interpretation.

Step 3: Add noise if required. Introduce a controlled noise source to represent simplified background fluctuations. Document its type, amplitude, bandwidth, and random seed where applicable so runs can be compared reproducibly.

Step 4: Build the signal-processing path. Sample the selected output, apply the fourth-order Butterworth bandpass filter over 4–8 Hz, and calculate moving RMS. Verify the filter response and confirm that the feature rises when the band-limited signal magnitude rises.

Step 5: Construct the feedback controller. Compare y(t) with the reference r(t) = 0, calculate e(t), and connect the PID controller. Add saturation and anti-windup if supported by the chosen implementation. Verify the error sign and controller action with small test inputs before closing the loop.

Step 6: Implement stimulation. Generate the proposed 130 Hz charge-balanced biphasic pulse train. Define how controller output changes pulse amplitude or another parameter and impose explicit limits. Feed the stimulation current into the neural plant input.

Step 7: Test the closed loop. Run the same baseline conditions with feedback enabled. Compare neural traces and the RMS feature between open-loop and closed-loop cases. Test more than one gain or noise condition if time permits.

Step 8: Record evidence and assess performance. Save scopes and numerical data for v(t), filtered output, y(t), controller output, and stimulation waveform. Use consistent axes and simulation durations. Report measured reductions or failures only if supported by the saved data.

6.1 Evaluation measures

Change in moving RMS feature between open-loop and closed-loop runs, calculated over comparable steady-state intervals.
Time response of the neural state and filtered output, including transient behaviour.
Controller effort and stimulation amplitude, including whether the actuator saturates.
Sensitivity to noise and selected controller gains, if those tests are performed.
Numerical stability and repeatability of the simulation.

7. Results and Discussion:
   
7.1 Simulation setup
The FitzHugh-Nagumo plant was simulated with a = 0.7, b = 0.8, τ = 12.5. The time scale was calibrated so the pathological limit cycle oscillates at 6 Hz, inside the 4–8 Hz tremor band. The healthy state (I_ext = 0) is a stable focus. A step in I_ext to 0.5 at t = 2 s crosses the Hopf bifurcation (analytically near I ≈ 0.33) and produces sustained oscillation. Simulations ran for 20 s with a fixed-step RK4 solver (0.1 ms) and 1 kHz digital sensing. Three scenarios were compared under identical noise seeds: (i) no stimulation, (ii) open-loop continuous stimulation at constant amplitude, and (iii) closed-loop PID-controlled stimulation.

<img width="1359" height="569" alt="Screenshot 2026-11-02 at 10 25 15 PM" src="https://github.com/user-attachments/assets/55d491d7-68f8-4bd4-bdd7-8ad240fb8635" />

7.2 Uncontrolled pathological dynamics

Without stimulation, the membrane potential remains at a quiescent rest state with noise-driven fluctuations until t = 2 s. After the onset step, v(t) enters a sustained relaxation oscillation at about 6 Hz. The biomarker y(t) rises to [X] and remains there. The power spectrum of v(t) shows a dominant peak inside the 4–8 Hz band (Fig. 3). This confirms that the plant reproduces a tremor-like state and that the band-pass/RMS chain detects it.

<img width="1283" height="750" alt="Screenshot 2026-11-02 at 10 27 14 PM" src="https://github.com/user-attachments/assets/4eb85894-922e-46f9-8857-d23f4b9162c6" />

7.3 Closed-loop response

Healthy period (0–2 s): the controller output is u = 0 and no stimulation is delivered, apart from a brief initial transient from the integrator.

Onset (t = 2 s): v(t) shows a single large excursion and y(t) rises to about 0.8. The PID command saturates at the lower limit (−1.5) within roughly 0.3–0.5 s, and the pulse envelope grows accordingly. The oscillation is suppressed within about [X] s and y(t) returns close to the noise floor.

Maintenance phase: with the tremor suppressed, y(t) falls below the setpoint (r = 0.05), the error becomes positive, and the integrator gradually releases. The command relaxes from −1.5 toward about −0.25, and the stimulation envelope shrinks.

Relapses: once stimulation falls to roughly the Hopf threshold, the tremor re-emerges (at about 7.7 s and 14.5 s). The controller re-engages within about 0.5 s and suppresses it again. Smaller partial bursts near 6 s and 13 s were caught early.

This repeating cycle is expected. The pathological drive I_ext = 0.5 persists, and the net effective bias is approximately I_ext + 0.7·u, so u ≈ −0.25 places the system at the Hopf point (0.5 − 0.175 ≈ 0.33). The controller therefore finds and tracks the minimum stimulation that holds the tremor off, and noise-induced precursors near the bifurcation trigger the periodic relapses. The relapse interval of about 6–7 s is set by the slow integrator release rate (≈ K_i·r_ref ≈ 0.15 s⁻¹) compared with the fast engagement rate.

A first run with r = 0 showed a design flaw. Because y ≥ 0, the error could never become positive, so the integrator never released and the stimulation did not throttle down even in the healthy state. Introducing a small positive setpoint just above the noise floor removed this problem.

7.6 Limitations

1. The plant is a single excitable unit, not a coupled population. Real pathological synchrony is a network property (e.g., STN-GPe loops), so the model is a proxy for population rhythm.
High-frequency stimulation effects in tissue (depolarization block, synaptic depression, desynchronization) are not captured. A mean-field term (stim_dc = 0.7) represents the net change
in excitability.

2. Purely zero-mean biphasic pulses have little effect on this reduced model. As a result, the actuator is not strictly charge-balanced.

3. Sensing adds about 0.2 s of lag (filter group delay plus the 250 ms RMS window), which limits achievable loop speed and contributes to the relapse cycle.
   
4. The stimulation energy metric (Σ I²·dt) is in arbitrary units and is not calibrated to clinical electrode impedance or pulse width.
   
5. PID gains were hand-tuned, and no formal stability margins were derived. The threshold-like plant nonlinearity makes linear analysis valid only locally.

Caveats

1. The relapses come about 6–7 s apart because Ki is small. A larger Ki or a smaller Kp results in faster, smaller probing, and the tremor returns more often with lower relapse amplitude. Larger Ki also gives a more oscillatory loop, so check the plots. A larger r_ref allows more residual tremor but reduces stimulation use. Pick whichever trade-off you want to show.

2. v is slightly noisy between bursts. That is the intended background noise, not the tremor.





                                       







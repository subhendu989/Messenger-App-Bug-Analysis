# Case Study: Investigating a Device & Feature-Specific UI Bug in Facebook Messenger

## 📌 About The Project
This repository documents a real-world software testing case study focused on mobile application compatibility and layout regression. It highlights an isolated **UI Overlapping & Layout Constraint Failure** discovered in the Facebook Messenger Android application during exploratory testing. 

This specific bug serves as an excellent example of a defect that is highly isolated, failing only under a unique combination of user action (**Feature-Specific**) and smartphone hardware/OS (**Device-Specific**).

---

## 📖 Project Description
While testing the Facebook Messenger app on a **Vivo Y50** device, a critical layout failure was observed during the outgoing call state. To determine whether the issue was global or isolated, a structured **Cross-Device** and **Cross-Feature Validation** was conducted:

1. **Cross-Feature Validation:** Initiated a standard *Audio Call* on the same Vivo Y50 device ➡️ The UI rendered **perfectly**.
2. **Cross-Device Validation:** Initiated a *Video Call* on other OEM devices (e.g., Samsung, Xiaomi) ➡️ The UI rendered **perfectly**.

### 💡 Root Cause Analysis (QA Perspective)
The defect is uniquely triggered when the application attempts to initialize the front-camera preview layer alongside the interactive UI control overlay during the "Calling..." state. The dynamic layout script fails to correctly calculate the screen width and safe area padding under Vivo's customized **Funtouch OS** framework combined with the device's **19.5:9 aspect ratio**. As a result, all secondary UI elements fall back to a default left-aligned coordinate, causing a severe overlap.

---

## 📝 Bug Report

* **Bug ID:** MSG-UI-026
* **Title:** UI control elements shift left and overlap during outgoing Video Calls specifically on Vivo Y50.
* **Severity:** Medium (Impairs usability and accessibility of specific video features)
* **Priority:** Medium
* **Environment:** Vivo Y50 (Android / Funtouch OS), Facebook Messenger (Latest Production Build).
* **Pre-condition:** The active state must be an outgoing **Video Call**. The bug does not reproduce during Audio Calls or on other tested non-Vivo hardware configurations.

### Steps to Reproduce:
1. Launch the **Facebook Messenger** app.
2. Select any contact from the chat list.
3. Tap on the **Video Call** icon to initiate an outgoing video call.
4. Observe the bottom utility and feature bar (*Touch up, Blur, Effects*, etc.) during the "Calling..." screen state.

### Actual Result:
The primary action button (End Call) is correctly aligned and centered. However, all secondary media options, filters, and camera control icons collapse to the left margin, stacking heavily on top of one another.

### Expected Result:
All UI control elements should distribute evenly across the screen alignment grid without any overlapping, mirroring the stable and correct layout seen during standard Audio Calls.

---

## 🖼️ Visual Evidence

Below is the screenshot capturing the exact state of the layout failure.

<p align="center">
  <img src="screenshots/messenger-ui-overlapping-vivo-y50.jpg" width="350" alt="Messenger UI Overlapping Bug Screen"/>
  <img src="screenshots/messenger-ui-overlapping-vivo-y50.jpg" width="350" alt="Messenger UI Overlapping Bug Screen"/>
</p>

---

## 🛠️ QA Engineering Takeaway
This case study is a textbook real-world example of why **Compatibility Testing** and **State-Combination Testing** are non-negotiable in mobile application Quality Assurance. 

Testing solely on clean or stock Android configurations is insufficient; OEM-customized skins (like Funtouch OS, MIUI, One UI) and variable display aspect ratios frequently introduce environmental edge cases. Validating different feature combinations (such as Audio state vs. Video state change) is vital to ensure an uninterrupted user experience across a fragmented device ecosystem.

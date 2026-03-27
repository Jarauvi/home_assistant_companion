# Home Assistant Companion for Android
**With experimental Finnish Wake Word Detection for Home Automation.**

# Description
This repository contains Home Assistant Companion fork with added custom wakewords for microWakeWord

There are currently these wakewords:
- "Hei koti"
- "Töttöröö"
- "Kuule pekka"

*Fun fact: "Kuule Pekka", because my Assistant personality is Pekka Puupää and Piper voice is Esa Pakarinen's voice, so this makes it truly authentic Savonian AI assistant.*

If you would like to have your custom wakeword included, you can raise an issue to propose it.
This fork should compile the latest Home Assistant Companion embedding the models each time new official release is published

## The boring stuff - research notes
### Abstract
This experiment aimed to identify a robust, culturally relevant, and phonetically distinct Finnish wake word for a local voice assistant. Three candidates were tested: "Hei Koti", "Töttöröö", and "Kuule Pekka". Through iterative training and phonetic stress testing, "Kuule Pekka" was identified as the superior model, achieving a 92.34% recall with a False Acceptance Per Hour (FAPH) of 0.72

### Candidate Selection
The candidates were selected based on three distinct phonetic profiles:

| Phrase | Phonetic Logic | Result |
| :--- | :--- | :--- |
| **"Hei Koti"** | Natural language, easy to say. | **FAIL:** Too many false positives from common Finnish greetings and TV dialogue. |
| **"Töttöröö"** | High phonetic uniqueness using "Ö" vowels. | **FAIL:** High False Rejection Rate (FRR). TTS models struggled to produce natural variations, leading to an "overfit" model. |
| **"Kuule Pekka"**| Hard consonants (K, P) and rhythmic structure. | **SUCCESS:** Exceptional structural anchors for the neural network to "lock" onto. |

 ### Methodology
 The dataset was synthesized in Kaggle using **Piper TTS** to ensure infinite data variety.

 #### Dataset Composition (Total: 5,172 samples)
* **Voice Models:** 4 distinct self trained Finnish Piper voices.
* **Sample variations:** Every sample was processed with normal + randomized pitch shifts and generation parameters to simulate large amount of different voices and speaker styles
* **Positive Samples (3,600):** A weighted mix of *"Kuulepas Pekka"* (50%), *"Kuule Pekka"* (30%), and *"Hei Pekka"* (20%).
* **Negative Samples (1,572):** General Finnish speech and "Hard Negatives" (Near-misses like*Kuule Pappa*, or *Haipakka*).
* **MicroWakeWord Negative Samples:** To extend the negative split of dataset, I added the original data used in MicroWakeWord training
* **Augmentation**: RIR for different room acoustics and ESC-50 for background noise was used in dataset.

### Training Logs & Performance
Training was conducted using the `mww7` architecture for 10,000 steps.

#### The "Sensitivity Cliff"
During Step 8,000, the model experienced a "Weight Collapse," where false positives spiked to **212.81 per hour**. Continued training was required to reach the stable "Champion State" at Step 9500.

**Step 9500 Metrics:**
* **Recall at No FAPH:** 70.58%
* **Average Viable Recall:** 92.34%
* **Estimated False Positives:** 0.72/hr
* **Precision:** 99.77%

### Deployment Findings
Testing on hardware revealed that while the model suggests a cutoff of 1.00, real-world acoustics require a lower threshold.

| Setting | User Experience |
| :--- | :--- |
| **0.96** | Zero false triggers, requires very clear pronunciation. |
| **0.92** | Best balance of ease-of-use and stability. |
| **0.44** | Wakes up if you even look at it; too sensitive. |

### Conclusion
The transition from simple greetings ("Hei Koti") to imperative commands with strong consonants ("Kuulepas Pekka") reduced false triggers by over **400%**. The use of a weighted phrase system allows the model to remain flexible while maintaining a high degree of certainty on the primary wake word.

---

# Home Assistant Companion for Android

[![Build Status](https://github.com/home-assistant/android/actions/workflows/onPush.yml/badge.svg)](https://github.com/home-assistant/android/actions/workflows/onPush.yml)  
[![Play Store](https://img.shields.io/badge/Play%20Store-Download-blue?logo=google-play)](https://play.google.com/store/apps/details?id=io.homeassistant.companion.android)
[![Play Store Beta](https://img.shields.io/badge/Play%20Store%20Beta-Download-blue?logo=google-play)](https://play.google.com/apps/testing/io.homeassistant.companion.android)
[![Discord](https://img.shields.io/discord/330944238910963714?label=Discord&logo=discord)](https://discord.gg/c5DvZ4e)
[![Stars](https://img.shields.io/github/stars/home-assistant/android?style=social)](https://github.com/home-assistant/android/stargazers)

Welcome to the **Home Assistant Companion for Android**! This is the official Android app for [Home Assistant](https://www.home-assistant.io/), a powerful open-source home automation platform. Join us in building an app used by millions of users worldwide.

---

## Features

- **Control Your Smart Home**: Seamlessly interact with your Home Assistant instance.
- **Native Android Experience**: Leverage Android-specific features like widgets, notifications, and location tracking.
- **Customizable**: Tailor the app to your needs with themes, dashboards, and more.
- **Open Source**: Contribute to a project that empowers users to take control of their smart homes.

## Get the app

- **[Download from the Play Store](https://play.google.com/store/apps/details?id=io.homeassistant.companion.android)**  
  Join the [Play Store Beta](https://play.google.com/apps/testing/io.homeassistant.companion.android) to test new features early.
- **Other Stores**: The app is also available in other app stores.

## Documentation

Looking for help? Check out the [Home Assistant Companion Documentation](https://companion.home-assistant.io/) for detailed guides on using the app.

## Report a bug or request a feature

Found a bug or have an idea for a new feature? Let us know!  

- **[Open a Bug Report](https://github.com/home-assistant/android/issues/new?template=Bug_report.md)**  
- **[Request a Feature](https://github.com/home-assistant/android/issues/new?template=feature_request.md)**  

We appreciate your feedback and contributions to make the app even better!

## Contributing

We are thrilled to welcome contributions from the community! This app exists thanks to the incredible efforts of the Home Assistant community. Whether you're fixing bugs, adding new features, or improving documentation, your contributions make a difference.

Every contribution, big or small, is greatly appreciated. Together, we can make the Home Assistant Companion for Android even better!

### Getting started

1. Read the [Developer Guide](https://developers.home-assistant.io/docs/android/).
2. Fork the repository and create a branch for your changes.
3. Submit a pull request and join the discussion!

## Join the community

Connect with other contributors and users in our vibrant **[Discord Community](https://discord.gg/c5DvZ4e)**: Join the **[#Android](https://discord.com/channels/330944238910963714/1346948551892009101)** channel to chat with developers and contributors.

## Star the repository

If you find this project useful, consider giving it a star on GitHub!  
It helps others discover the project and motivates us to keep improving.

<a href="https://next.ossinsight.io/widgets/official/analyze-repo-stars-history?repo_id=179008173" target="_blank" style="display: block" align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://next.ossinsight.io/widgets/official/analyze-repo-stars-history/thumbnail.png?repo_id=179008173&image_size=auto&color_scheme=dark" width="721" height="auto">
    <img alt="Star History of home-assistant/android" src="https://next.ossinsight.io/widgets/official/analyze-repo-stars-history/thumbnail.png?repo_id=179008173&image_size=auto&color_scheme=light" width="721" height="auto">
  </picture>
</a>

[![Home Assistant - A project from the Open Home Foundation](https://www.openhomefoundation.org/badges/home-assistant.png)](https://www.openhomefoundation.org/)

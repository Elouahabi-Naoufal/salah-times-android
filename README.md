<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

<!-- PROJECT SHIELDS -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- PROJECT TITLE -->
<br />
<div align="center">
  <a href="https://github.com/Elouahabi-Naoufal/salah-times-android">
    <img src="salah_times.png" alt="Salah Times app screenshot" width="229" height="397">
  </a>

  <h3 align="center">Salah Times for Android</h3>

  <p align="center">
    Native Android prayer times for 419 cities in 38 countries, with an offline SQLite cache,
    a live next-prayer countdown, Adhkar and prayer notifications.
    <br />
    <a href="https://github.com/Elouahabi-Naoufal/salah-times-android"><strong>Explore the repository »</strong></a>
    <br />
    <br />
    <a href="https://github.com/Elouahabi-Naoufal/salah-times-android/issues">Report Bug</a>
    &middot;
    <a href="https://github.com/Elouahabi-Naoufal/salah-times-android/issues">Request Feature</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

Salah Times is a native Java Android app that shows the five daily prayers for a chosen city,
counts down to the next one and keeps working without a network after the first successful fetch.

The data path is cache-first. `PrayerTimesService` fetches the monthly prayer table for a city from
[yabiladi.com](https://www.yabiladi.com/prieres/), parses the HTML with Jsoup, and stores every row
in a local SQLite table (one table per city) through `DatabaseHelper`/`StorageManager`. On later
launches the UI reads today's row from SQLite; if the cached row is stale it refreshes in the
background. `ScrapeAllCitiesService` can populate multiple cities in one foreground service run.

Implemented screens and behaviour:

* **Main screen** — today's prayer cards, a live clock, the current prayer highlight and a
  `HH:MM:SS` countdown to the next prayer.
* **City selection** — searchable city list built from `CitiesData`, which contains 419 cities
  across 38 countries (43 in Morocco) with English, Arabic and French names.
* **Settings** — persistent city, language and notification preferences.
* **Languages** — English, Arabic, French and Spanish through `TranslationManager`.
* **Adhkar** — a list of remembrances with add/edit support.
* **Notifications and alarms** — notification channels, exact-alarm scheduling, a boot receiver
  and a foreground data-sync service.
* **Hijri date** — a calculated Hijri date in `HijriDateManager` (an approximation based on the
  Gregorian year, not an API lookup).

The repository root also contains the original Python desktop script (`ultra_modern_salah.py`), a
Flutter `pubspec.yaml`/`lib/` scaffold and a `docs/` directory. **Only `app/` is part of the Android
Gradle build**; the other trees are historical or experimental.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

* [![Java][java-shield]][java-url]
* [![Android][android-shield]][android-url] — `compileSdk`/`targetSdk` 36, `minSdk` 30
* [![Gradle][gradle-shield]][gradle-url] — Android Gradle Plugin 8.13.1, Kotlin DSL
* [![SQLite][sqlite-shield]][sqlite-url] — via `android.database.sqlite`
* [![AndroidX][androidx-shield]][androidx-url] — AppCompat, Material Components, RecyclerView
* [![Jsoup][jsoup-shield]][jsoup-url] — HTML parsing for the prayer-time source

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

### Prerequisites

* Android Studio with SDK 36 installed.
* JDK 11 or newer (the module compiles with Java 11 compatibility).
* A device or emulator running Android 11 (API 30) or newer.
* Internet access for the first prayer-time fetch; no API key is required.

### Installation

1. Clone the repository
   ```sh
   git clone https://github.com/Elouahabi-Naoufal/salah-times-android.git
   cd salah-times-android
   ```
2. Build a debug APK
   ```sh
   ./gradlew assembleDebug
   ```
   The APK is written under `app/build/outputs/apk/debug/`.
3. Install it on a connected device
   ```sh
   ./gradlew installDebug
   ```
   Or open the project in Android Studio and press Run. The launcher activity is
   `com.salah.times.MainActivity` (application ID `android.Salah_App`).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE -->
## Usage

1. On first launch, pick a country and a city. The app fetches the current month for that city and
   writes it to the local database.
2. The main screen shows today's Fajr, Dohr, Asr, Maghreb and Isha times, the next prayer and a
   countdown.
3. Use the refresh control to re-fetch. With no connection, cached days continue to display.
4. Open Settings to change the city, language or notification behaviour, and Adhkar for the
   remembrances list.
5. Grant notification and exact-alarm permissions when prompted; the boot receiver re-registers
   alarms after a reboot.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ROADMAP -->
## Roadmap

- [x] Native Android UI with Material components
- [x] Monthly scraping and local SQLite cache
- [x] Next-prayer countdown, notifications and boot persistence
- [x] City database with English/Arabic/French names
- [x] Adhkar list and four-language UI
- [ ] Replace the approximate Hijri calculation with an API or verified calendar source
- [ ] Remove the unused Python/Flutter material from the repository root
- [ ] Add unit tests for date parsing and cache refresh logic
- [ ] Add a Gradle task or CI workflow that builds the APK automatically

See the [open issues](https://github.com/Elouahabi-Naoufal/salah-times-android/issues) for a full list of proposed features and known issues.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Top contributors:

<a href="https://github.com/Elouahabi-Naoufal/salah-times-android/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Elouahabi-Naoufal/salah-times-android" alt="contrib.rocks image" />
</a>

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
## License

No `LICENSE` file is bundled with this repository. This project is not licensed — all rights reserved.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->
## Contact

Naoufal Elouahabi
* LinkedIn: [https://www.linkedin.com/in/elouahabi-naoufal/](https://www.linkedin.com/in/elouahabi-naoufal/)
* Email: [elouahabi-naoufal1@gmail.com](mailto:elouahabi-naoufal1@gmail.com)

Project Link: [https://github.com/Elouahabi-Naoufal/salah-times-android](https://github.com/Elouahabi-Naoufal/salah-times-android)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [yabiladi.com](https://www.yabiladi.com/prieres/) for the published prayer-time tables
* [Jsoup](https://jsoup.org/)
* [Material Design 3](https://m3.material.io/)
* [Best-README-Template](https://github.com/othneildrew/Best-README-Template)
* [Img Shields](https://shields.io)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/Elouahabi-Naoufal/salah-times-android.svg?style=for-the-badge
[contributors-url]: https://github.com/Elouahabi-Naoufal/salah-times-android/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/Elouahabi-Naoufal/salah-times-android.svg?style=for-the-badge
[forks-url]: https://github.com/Elouahabi-Naoufal/salah-times-android/network/members
[stars-shield]: https://img.shields.io/github/stars/Elouahabi-Naoufal/salah-times-android.svg?style=for-the-badge
[stars-url]: https://github.com/Elouahabi-Naoufal/salah-times-android/stargazers
[issues-shield]: https://img.shields.io/github/issues/Elouahabi-Naoufal/salah-times-android.svg?style=for-the-badge
[issues-url]: https://github.com/Elouahabi-Naoufal/salah-times-android/issues
[license-shield]: https://img.shields.io/badge/License-All%20rights%20reserved-red.svg?style=for-the-badge
[license-url]: https://github.com/Elouahabi-Naoufal/salah-times-android
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/elouahabi-naoufal/
[java-shield]: https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white
[java-url]: https://openjdk.org/
[android-shield]: https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white
[android-url]: https://developer.android.com/
[gradle-shield]: https://img.shields.io/badge/Gradle-02303A?style=for-the-badge&logo=gradle&logoColor=white
[gradle-url]: https://gradle.org/
[sqlite-shield]: https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white
[sqlite-url]: https://www.sqlite.org/
[androidx-shield]: https://img.shields.io/badge/AndroidX-3DDC84?style=for-the-badge&logo=android&logoColor=white
[androidx-url]: https://developer.android.com/jetpack/androidx
[jsoup-shield]: https://img.shields.io/badge/Jsoup-4A90D9?style=for-the-badge&logo=java&logoColor=white
[jsoup-url]: https://jsoup.org/

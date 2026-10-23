# EdusmartAI
name: Build EduSmart AI APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Ambil proyek
        uses: actions/checkout@v4

      - name: Siapkan Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Siapkan Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Siapkan Android SDK
        uses: android-actions/setup-android@v3

      - name: Pasang Android SDK
        run: |
          sdkmanager "platforms;android-35" "build-tools;35.0.0"

      - name: Ekstrak proyek
        run: unzip EduSmartAI_Android_Project.zip -d source

      - name: Build APK
        run: gradle -p source/EduSmartAI --no-daemon assembleDebug

      - name: Simpan APK
        uses: actions/upload-artifact@v4
        with:
          name: EduSmartAI-APK
          path: source/EduSmartAI/app/build/outputs/apk/debug/app-debug.apk

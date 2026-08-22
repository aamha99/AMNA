name: Build Walayh APK

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: 📥 استنساخ المستودع (Checkout Repository)
      uses: actions/checkout@v4

    - name: ☕ إعداد بيئة جافا (Set up JDK 17)
      uses: actions/setup-java@v4
      with:
        distribution: 'temurin'
        java-version: '17'

    - name: 🛠️ منح صلاحيات التنفيذ لـ Gradle
      run: chmod +x gradlew

    - name: 🚀 بناء ملف الـ APK (Build Debug APK)
      run: ./gradlew assembleDebug

    - name: 📦 رفع ملف الـ APK الناتج (Upload APK Artifact)
      uses: actions/upload-artifact@v4
      with:
        name: Walayh-Debug-APK
        path: app/build/outputs/apk/debug/app-debug.apk

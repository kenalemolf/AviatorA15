# aviatorbotA15
push and build APK
name: Build APK for AviatorBotA15

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: 3.10

    - name: Install Buildozer and dependencies
      run: |
        sudo apt update
        sudo apt install -y zip unzip openjdk-17-jdk python3-pip \
          build-essential git python3-dev libffi-dev libssl-dev \
          libsqlite3-dev libjpeg-dev zlib1g-dev libfreetype6-dev \
          libgl1-mesa-dev

        pip install --upgrade pip
        pip install cython buildozer

    - name: Extract the project ZIP
      run: |
        unzip kivy_samsung_a15_bot.zip -d aviator
        cd aviator && buildozer android debug

    - name: Upload APK
      uses: actions/upload-artifact@v3
      with:
        name: AviatorBotA15-APK
        path: aviator/bin/*.apk

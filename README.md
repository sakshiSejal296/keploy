name: Keploy Test

on:
  pull_request:
    branches:
      - main

jobs:
  keploy_test_case:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm install

      - name: Run Keploy Tests
        run: |
          curl --silent --location "https://github.com/keploy/keploy/releases/latest/download/keploy_linux_amd64.tar.gz" | tar xz --overwrite -C /tmp
          sudo mkdir -p /usr/local/bin && sudo mv /tmp/keploy /usr/local/bin/keploy
          keploy test -c "node src/app.js"

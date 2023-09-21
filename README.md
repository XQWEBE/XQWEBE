- 👋 Hi, I’m @XQWEBE
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
XQWEBE/XQWEBE is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
name: Itch.io release

on:
  release:
    types: [published]
  workflow_dispatch:

jobs:
  itch-release:
    runs-on: ubuntu-latest
    steps:
      - name: Fetch Github Release Asset
        id: fetch-release
        uses: dsaltares/fetch-gh-release-asset@1.0.0
        with:
          repo: "pattlebass/Music-DJ"
          regex: true
          file: "MusicDJ.*"
          target: "./"
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Echo files
        run: |
          echo "Files:"
          ls
          echo "Version name: ${{ steps.fetch-release.outputs.version }}"
          echo "Release name: ${{ steps.fetch-release.outputs.name }}"
          echo "Body: ${{ steps.fetch-release.outputs.body }}"

      - name: Butler Push Linux
        uses: manleydev/butler-publish-itchio-action@v1.0.3
        env:
          BUTLER_CREDENTIALS: ${{ secrets.BUTLER_API_KEY }}
          CHANNEL: linux
          VERSION: ${{ steps.fetch-release.outputs.version }}
          ITCH_GAME: musicdj
          ITCH_USER: pattlebass
          PACKAGE: MusicDJ.Linux.zip

      - name: Butler Push Windows
        uses: manleydev/butler-publish-itchio-action@v1.0.3
        env:
          BUTLER_CREDENTIALS: ${{ secrets.BUTLER_API_KEY }}
          CHANNEL: windows
          VERSION: ${{ steps.fetch-release.outputs.version }}
          ITCH_GAME: musicdj
          ITCH_USER: pattlebass
          PACKAGE: MusicDJ.Windows.zip

      - name: Butler Push Android 32bit
        uses: manleydev/butler-publish-itchio-action@v1.0.3
        env:
          BUTLER_CREDENTIALS: ${{ secrets.BUTLER_API_KEY }}
          CHANNEL: android-32bit
          VERSION: ${{ steps.fetch-release.outputs.version }}
          ITCH_GAME: musicdj
          ITCH_USER: pattlebass
          PACKAGE: MusicDJ_32bit.apk

      - name: Butler Push Android 64bit
        uses: manleydev/butler-publish-itchio-action@v1.0.3
        env:
          BUTLER_CREDENTIALS: ${{ secrets.BUTLER_API_KEY }}
          CHANNEL: android-64bit
          VERSION: ${{ steps.fetch-release.outputs.version }}
          ITCH_GAME: musicdj
          ITCH_USER: pattlebass
          PACKAGE: MusicDJ_64bit.apk

      - name: Butler Push HTML5
        uses: manleydev/butler-publish-itchio-action@v1.0.3
        env:
          BUTLER_CREDENTIALS: ${{ secrets.BUTLER_API_KEY }}
          CHANNEL: html5
          VERSION: ${{ steps.fetch-release.outputs.version }}
          ITCH_GAME: musicdj
          ITCH_USER: pattlebass
          PACKAGE: MusicDJ.HTML5.zip

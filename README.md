GitHub action to download a file from Google Drive. This action authenticates
to Google with an access token with at least the scope:
  `https://www.googleapis.com/auth/drive.readonly`

File ID's can be specified by either the raw id on Google Drive, or by using
the URL obtained from `Get URL` from the Google Drive dashboard.

A suitable token for a Google Service account can be obtained with the action
provided by https://github.com/PlayEveryWare/action-google-access-token

Inputs
======
* token
  An access token with read access to the target Google Drive:w

* file-id
  The raw File ID for the file to download

* file-url
  The URL to a Google Drive shared link

* path
  Target path for the downloaded file. This file will be removed by the
  action's cleanup step.

Outputs
=======
_none_

Example
=======

Using raw File ID
-----------------
```yaml
jobs:
  example_google_drive_download:
    runs-on: ...
    steps:
      - name: Acquire Google access token
        id: google-access-token
        uses: playeveryware/action-google-access-token@v1
        with:
          credentials: ${{ secrets.GOOGLE_SERVICE_ACCOUNT_CREDENTIALS }}
          scope: 'https://www.googleapis.com/auth/drive.readonly'

      - name: Download file
        uses: playeveryware/action-google-drive-download@v1
        with:
          token: ${{ steps.google-access-token.outputs.token }}
          file-id: 1awy6ufig25dGHLyPRaz_Fkd7c1930NS-
          path: test.bin

      - name: Show file information
        run: |
          ls test.bin
```

Using a shared URL
------------------
```yaml
jobs:
  example_google_drive_download:
    runs-on: ...
    steps:
      - name: Acquire Google access token
        id: google-access-token
        uses: playeveryware/action-google-access-token@v1
        with:
          credentials: ${{ secrets.GOOGLE_SERVICE_ACCOUNT_CREDENTIALS }}
          scope: 'https://www.googleapis.com/auth/drive.readonly'

      - name: Download file
        uses: playeveryware/action-google-drive-download@v1
        with:
          token: ${{ steps.google-access-token.outputs.token }}
          file-url: 'https://drive.google.com/file/d/1awy6ufig25dGHLyPRaz_Fkd7c1930NS-/view?usp=sharing'
          path: test.bin

      - name: Show file information
        run: |
          ls test.bin

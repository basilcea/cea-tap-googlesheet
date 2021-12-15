# tap-cea-googlesheet

`tap-cea-googlesheet` is a Singer tap for googlesheet.

Built with the [Meltano Tap SDK](https://sdk.meltano.com) for Singer Taps.

## Installation

```bash
pip3 install pix

pipx ensurepath

pipx install poetry

git clone https://github.com/basilcea/cea-tap-googlesheet.git

poetry install
```

## Configuration

### Accepted Config Options

Create an `.env` file in the root folder and add the following configuration in the format

```bash
export TAP_CEA_GOOGLESHEET_CLIENT_ID=<your client -id>

```

The following configuration are used by  `tap-cea-googlesheet`:
| Values | Description | Type |
| --- | ---| ---|
|`TAP_CEA_GOOGLESHEET_CLIENT_ID` | The client id generated from google credentials | `Required` |
|`TAP_CEA_GOOGLESHEET_CLIENT_SECRET` | The client secret from google credentials | `Required` |
| `TAP_CEA_GOOGLESHEET_SPREADSHEET_ID`| The spreadsheet to be read found at the docs sheet immediately after `https://docs.google.com/spreadsheets/d/` |`Required`|

#### Optional Config - Sheet_ranges
in your `meltano.yml`file,  in the `config` section,  specify your sheet ranges, an array of ranges to be read in the format SHEETNAME!A1:B2 where A1:B2 is the range choice. Adding it makes the extraction faster . If a sheet does not have the first rows, then it is required to specify the range to be read for that sheet.


### Source Authentication and Authorization

- To use this plugin, you need your google credentials. To get your google credentials file , follow the instructions in the link: [Getting your Credentials](https://support.google.com/googleapi/answer/6158849?hl=en&ref_topic=7013279)

After creating your projects and a `Desktop App` credential
- Select the API & Services
- Enable Sheets API and Drive API
- Use the credential you created for both API
- Go to the OAuth Consent Screen tab on the API & Services Menu. And Click on the Project to Add Scope

  Add the following scopes:
```bash
  'https://www.googleapis.com/auth/drive.readonly'
  'https://www.googleapis.com/auth/drive.metadata.readonly'
  'https://www.googleapis.com/auth/drive.file'
  'https://www.googleapis.com/auth/spreadsheets.readonly
```
- Download your credentials file which has your client secret and client id or copy both
- Set them in the  `.env` file for use by the extractor.


## Usage

You can easily run `tap-cea-googlesheet` by itself or in a pipeline using [Meltano](https://meltano.com/).


## Developer Resources

### Create and Run Tests

Create tests within the `tap_cea_googlesheet/tests` subfolder and
  then run:

```bash
poetry run pytest
```

You can also test the `tap-cea-googlesheet` CLI interface directly using `poetry run`:

```bash
poetry run tap-cea-googlesheet --help
```
### Testing Locally with Poetry

Create a config.json file using the accepted configurations

```bash
{
    "client_id":"<client_id from google credentials>",
    "client_secret":"<client secret from google credentials>",
    "spreadsheet_id":"<sheet id to be read found after spreadsheets/d/ in the google sheets url>",
    "sheet_ranges": "<array of ranges  to be read>"
}
```
```bash
# Test invocation
poetry run tap-googlesheet --config <your config file path>.json

# Check discovery

poetry run tap-googlesheet --config <your config file path>.json --discover

# Print Output

poetry run tap-googlesheet  --config <your config file path>.json > out.jsonl

```

### Testing with [Meltano](https://www.meltano.com)

_**Note:** This tap will work in any Singer environment and does not require Meltano.
Examples here are for convenience and to streamline end-to-end orchestration scenarios._

Your project comes with a custom `meltano.yml` project file already created. Open the `meltano.yml` and follow any _"TODO"_ items listed in
the file.

Next, install Meltano (if you haven't already) and any needed plugins:

```bash
# Install meltano
pipx install meltano
# Initialize meltano within this directory
cd tap-cea-googlesheet
meltano install
```

Now you can test and orchestrate using Meltano:

```bash
# Test invocation:
meltano invoke tap-cea-googlesheet --version
# Check Discover:
meltano invoke tap-googlesheet --discover
# OR run a test `elt` pipeline:
meltano elt tap-cea-googlesheet target-jsonl
```

### SDK Dev Guide

See the [dev guide](https://sdk.meltano.com/en/latest/dev_guide.html) for more instructions on how to use the SDK to 
develop your own taps and targets.

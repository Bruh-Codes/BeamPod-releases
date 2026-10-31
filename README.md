# BeamPod releases

Installers for BeamPod live in the [`releases/`](releases) folder, one folder per version.
Download `BeamPod_<version>_x64-setup.exe` and run it. Installed copies update themselves.

## How installed copies are controlled

BeamPod reads two things from this repository:

1. **`latest.json` in this branch** tells every copy that a new version exists and where to
   download it. Updating this file (with a signed installer in `releases/`) drops an update.
2. **`config.json` in this branch** is read at every launch. Edit it on GitHub to change behaviour
   for every installed copy, no new build needed:

| Field | What it does |
| --- | --- |
| `min_version` | Copies older than this must update before they can be used (for example `"0.3.3"`). `"0.0.0"` means no requirement. |
| `license_required` | `true` makes every copy ask for a license key. |
| `license_check_url` | Where keys are checked. BeamPod sends `POST {"key", "version"}` and expects `{"valid": true/false, "message": "..."}`. If this is empty, nobody is locked out. |
| `message` | A notice shown in every copy, such as "Update before Sunday". Empty shows nothing. |
| `offline_grace_days` | How long a license that was checked once keeps working without internet. |

Safety rules built into the app: if a PC is offline, a forced update is never enforced, and a
license that was valid keeps working for the grace period. The app never stops a live service
because of this file.

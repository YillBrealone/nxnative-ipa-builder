# Troubleshooting

Having trouble? Start with the first failed step in your GitHub Actions run. A completed IPA build doesn't guarantee that a game will start or play correctly.

**Before sharing a screenshot or log:** remove game links, keys links, tokens and other private information. Never upload or post your `prod.keys` file. The workflow's log redaction is helpful but not guaranteed to catch everything.

## The workflow doesn't appear

- Check that GitHub Actions is enabled in your repository.
- Check that the workflow is in `.github/workflows/` on the default branch.
- If you're using a fork, enable Actions in your own copy.
- If the GitHub website is awkward on an iPad, try Safari's **Request Desktop Website** or start the workflow from another device.

If **Run workflow** appears but a run doesn't start, check the selected branch and repository permissions. A run started by **Re-run jobs** uses the original run's workflow definition; to use an updated workflow, commit it and start a new run.

## A game or keys download fails

- Check that the file has finished uploading and that the runner can download it without signing in or answering a confirmation prompt.
- For a Google Drive folder, check that its contents are accessible and contain the intended packages.
- For a direct URL, make sure it downloads the file rather than opening a sharing page.
- For Switch 1 packages, check that your `Keys_URL` repository secret points to your own valid `prod.keys` file.
- Check that the archive or folder contains supported packages and that they are for the same game.

Don't paste private links into an issue or share them with someone helping you.

## Updates or DLC are missing

Include the matching update and DLC packages when you build. A Google Drive folder can contain multiple packages, subject to download and engine support.

If you enter a base game link in the run form, the workflow uses the update and DLC fields from that form; it doesn't automatically mix in old update or DLC secrets. Leave a field blank only if you don't want to include that content.

For Switch 2 `.nup` files, support for combining separate base, update and DLC packages has not been verified. A build completing doesn't prove that every update or DLC was included or works.

## Title ID or profile error

Make sure the base game is included and the package is complete. Leave `profile_id` blank for automatic detection unless you know the correct profile ID. If detection fails, check the game's title ID and the first relevant error in the log.

A game profile being present—or a survey finding one—does not prove that the game is playable on your device.

## The build fails

1. Open the run and find the **first failed step**.
2. Read the error and the nearby lines rather than only the final summary.
3. Check the `NxNative-Build-Logs` artifact if one was produced. Detailed builder errors can be in `build_ipa.log`.
4. Check the selected engine, your package versions, runner availability and free space. Large games may need substantially more runner space than the download size.
5. Before asking for help, remove sensitive details from logs and screenshots.

Engine branches and GitHub runner tools can change, so a workflow that worked before may need an update. Don't apply random shader, renderer or diagnostic fixes without checking that they match your engine and game.

## The IPA or download is missing

- **Survey Only** does not build an IPA. Select **Build IPA** for a full build.
- Check that the build reached the upload steps. A failed build may not produce an IPA.
- Download the GitHub artifact ZIP from the run summary and extract the `.ipa`.
- GoFile upload is attempted unless disabled with the repository variable `GOFILE_UPLOAD=false`. If it fails, look for the GitHub artifact instead. The GoFile link opens a download page, not necessarily a direct IPA URL.

A public repository's artifacts and a GoFile link should not be treated as private. A bundled IPA can contain game data.

## The IPA installs but the game won't run

Check the engine's device and OS requirements and the upstream notes for the exact game and update. Some titles are untested or need game-specific work. Switch 2 mobile support and the older-device engine are experimental.

If you selected **separate RomFS**, the IPA alone may not contain the game data. This workflow does not upload the separate image. You need an image matching your own game and build, in the location required by the selected engine and sideloading method—commonly `Documents/romfs.img` for engines that support it.

If a game shows a black screen, graphical errors or crashes, record the game version, device, OS, selected engine and when the problem occurs. Don't assume a fix for one game or engine will work for another.

## Controls aren't working as expected

Open **... > Control Mode** and try the mode suited to the game. Tap and Trackpad are intended for Handheld mode. Trackpad maps dragging to the left stick and a short tap to A; it doesn't provide all the controls some games need. Not every game supports touchscreen input, and control behaviour has not been verified for every game and device.

## Rebuilding without losing saves

Back up your saves and data before replacing or reinstalling an IPA. Updates and DLC generally require a rebuild. With LiveContainer, preserve the existing app entry and data container when appropriate; changing the app identity or installing as new can leave saves in a different container. A backup is still important, and save compatibility isn't guaranteed.

## Asking for help

Include the device and OS version, game and update version, engine option, failed step, and the relevant **redacted** error lines. Don't include keys, private links, game files or unreviewed logs.

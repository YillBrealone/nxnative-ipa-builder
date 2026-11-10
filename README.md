# nxnative IPA Builder

Build an iPhone or iPad IPA from your own Nintendo Switch game files using GitHub Actions. You can start the build from your phone or tablet—GitHub provides the Mac that does the work.

This is a community project, not an official Nintendo or Apple product. **A successful build doesn't guarantee that a game will run.** Compatibility depends on the game, its version, your device and the engine you choose.

## Before you start

You'll need:

- A GitHub account with Actions enabled.
- Your own legally obtained game files and, for Switch 1 games, your own matching `prod.keys`.
- Downloadable Google Drive links or direct HTTP/HTTPS links to your files.
- A compatible iPhone or iPad, plus a way to sign and install the unsigned IPA, such as a compatible LiveContainer setup.
- Enough storage for the download, installation and game data.

The workflow accepts `.nsp`, `.xci`, `.nsz`, `.xcz` and `.nup` packages, and attempts to extract supported RAR, ZIP and 7Z archives. Support still depends on the game and engine. Switch 2 builds on iPhone and iPad are **experimental**; combining separate Switch 2 base, update and DLC packages has not been verified.

## Set it up

1. **Fork this repository** to your own GitHub account and enable Actions if prompted. The workflow must be on your default branch to appear in the manual run menu.
2. Upload your game and any update or DLC to a location the runner can download from. A Google Drive folder can be used for multiple packages, such as DLC files.
3. For Switch 1 games, add your keys link as a repository secret:
   - Open **Settings > Secrets and variables > Actions**.
   - Choose **New repository secret**.
   - Name it `Keys_URL` and paste the download link to your own `prod.keys` file.
4. Open **Actions > NxNative - Generic IPA Builder > Run workflow**.
5. Choose an engine, enter your game link and any update or DLC links, then select **Run workflow**. Leave `profile_id` blank unless you know you need an override.
6. Open the new run to follow its progress. When it finishes, download the IPA from its summary.

You can also store game links in secrets named `Game_URL`, `Update_URL` and `DLC_URL`. If you enter a `game_url` in the form, the workflow uses the update and DLC fields from that form rather than old update or DLC secrets.

**Keep keys and private links out of the workflow form, repository files, issues and logs.** A shareable keys link is sensitive: anyone who has it may be able to download your keys. Secrets are not copied to other people's forks.

## Choosing an engine

| Option | Best starting point | Note |
| --- | --- | --- |
| **NX2Apple Patched** | Newer iPhones | The default option. |
| **Standard** | Newer iPhones and supported iPads | Uses upstream nxnative. |
| **Metal 3 + BC fallback** | Older devices, including the 2020 A12Z iPad Pro | Experimental; compatibility and performance are not guaranteed. |

Standard nxnative targets iOS/iPadOS 26 or later, an iPhone with an A17 Pro or newer, or an iPad with an M1 or newer. Check the selected engine's documentation for its current requirements. The older-device option does not guarantee support for any particular device or game.

## Download and install

The run uploads an unsigned IPA as a **GitHub artifact**. Download the artifact ZIP, extract the `.ipa`, then sign and install it using your chosen compatible sideloading method.

By default, the workflow also tries to upload the IPA to **GoFile**. The run summary links to a GoFile download page, not necessarily a direct IPA link. If GoFile fails, check the GitHub artifact.

**Outputs may not be private.** A bundled IPA can contain game data, and public-repository artifacts and GoFile links may be accessible to others. To disable GoFile, create a repository Actions variable named `GOFILE_UPLOAD` with the value `false`. The GitHub artifact is still uploaded.

## RomFS and build options

- **Bundle** is the simplest starting point: it requests an all-in-one IPA containing game data.
- **Separate** requests an IPA without bundled game data. This workflow **does not upload a separate RomFS image**. You'll need a matching image from your own game and must place it in the correct app data container; for supported engines this is commonly `Documents/romfs.img`.
- **Survey Only** runs a diagnostic instead of building an IPA. It cannot guarantee a game will run.
- **Shader cache** is off by default. If enabled, it may store game-derived data that readers of a public repository can access.

## Controls

Use the in-game **... > Control Mode** menu to choose **2D**, **3D**, **Tap** or **Trackpad**. Trackpad uses dragging for the left stick and a short tap for A; it doesn't supply every button some games need. Tap and Trackpad are intended for Handheld mode. Controls may behave differently between games.

The workflow is designed to start games immediately, with Handheld and UK English as defaults. Game language availability varies, and the language input can override the default. Source checks do not guarantee that every app resource or game will appear in English.

## If something goes wrong

For more help, see the [troubleshooting guide](TROUBLESHOOTING.md).

- **Download fails:** Check that the link is accessible to the runner and downloads the actual file or supported Google Drive folder.
- **Keys error:** Check the `Keys_URL` secret in your own repository. Never share the keys or their link.
- **Title ID error:** Make sure the base game is included. Leave `profile_id` blank for automatic detection unless you know the correct ID.
- **Build fails:** Open the first failed step and check the `NxNative-Build-Logs` artifact if one is available.
- **Game doesn't launch:** Check your device and OS, selected engine, game version and upstream game notes. An IPA building successfully doesn't prove the game is compatible.

Review logs before sharing them: redaction is best effort and may miss sensitive information. Updates and DLC generally require a rebuild; back up your saves and preserve the correct app identity and data container when reinstalling.

## Credits

This workflow builds on [seedds/nxnative](https://github.com/seedds/nxnative), [LNPANDY/NX2Apple](https://github.com/LNPANDY/NX2Apple), [widmonstertony/nxnative](https://github.com/widmonstertony/nxnative) and their contributors, as well as community guides and workflows.

This project is unofficial and is not affiliated with Nintendo or Apple. Please use your own legally obtained game files and keys.

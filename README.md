# ZipNest

An offline archive tool by [NoAuthZone](https://github.com/NoAuthZone).

Create and extract encrypted ZIP archives locally in your browser.

Save [**ZipNest.html**](ZipNest.html) and open it directly in a current desktop
**Chrome or Edge** on Windows, macOS or Linux. It is a single offline HTML file:
no Python, installation, account or server is required.

## Create an archive

1. Select **Create ZIP**, then choose a folder or individual files.
2. Leave **Export → Browser download** selected and enter a ZIP file name.
   For large archives, select **Direct to disk** and choose a destination outside the source folder.
3. Adjust **Archive options** as needed.
4. Enter and repeat your password, unless you selected **None** for encryption.
5. Select **Create encrypted ZIP** or **Create unencrypted ZIP**.

The finished ZIP downloads automatically. If the browser blocks the automatic
download, click **Download ZIP**. The link stays available until the next operation
or until you close the page. Your browser controls where downloads are saved and
how existing names are handled.

Browser download buffers the ZIP in memory. Both source data and ZIP output are
limited to **256 MiB**; processing needs additional memory. Use **Direct to disk**
for large files and folders. In that mode, existing destination names are rejected. Source files remain unchanged.
Settings survive switching between Create and Extract, but reset when the
page is reloaded. Passwords are not saved.

## Archive options

| Setting | Choices | Default |
|---|---|---|
| Compression method | Deflate; Store without compression | Deflate |
| Compression level | 1 Fastest; 3 Fast; 5 Normal; 7 Maximum; 9 Ultra | 5 Normal |
| Encryption | AES-256; AES-128; ZipCrypto; None | AES-256 |
| ZIP64 | Automatic; Always; Off | Automatic |
| Verify archive after saving | Read and check the saved archive | Off |

**Compression:** Deflate levels are applied by an embedded JavaScript compressor.
They are functional settings, not labels for the browser's fixed default level.
Higher levels use more processing time and may produce a smaller archive; a
smaller result is not guaranteed. Store is suitable for video, photos and other
already-compressed files. Its level selector is disabled. Encrypted empty files
use a tiny Deflate stream for compatibility with 7-Zip.

**Encryption:** AES-256 is recommended. AES-128 is available for compatible
tools. ZipCrypto is weak and should only be used for legacy compatibility.
None creates a ZIP that anyone can read; selecting it clears and hides the
password fields. Protected archives require at least 12 characters; prefer a
long, random passphrase. Lost passwords cannot be recovered.

**ZIP64:** Automatic uses the extension when needed. Always forces ZIP64.
Off uses classic ZIP limits and rejects jobs with files or a conservative
estimated archive size that may exceed roughly 4 GiB, or with 65,535 or more
entries. A compressible folder may be rejected conservatively with ZIP64 Off;
choose Automatic in that case.

**Verification:** When selected, the completed archive is read (before download in Browser download mode) and its file
contents checked. This adds another read/decompression pass. Cancellation or
an error attempts to remove the newly created incomplete or failed output.

## Extract an archive

Choose **Extract archive**, select the archive, destination and a new output
folder name. Enter its password, or leave the field empty for an unencrypted
ZIP. All file contents are checked before plaintext files are written, then
the archive is read again to extract them. Creation options are hidden during
extraction, since the archive already specifies its compression and encryption.

Supported: ZIP with Store/Deflate compression, AES and ZipCrypto encryption,
unencrypted ZIPs, and legacy `.tresorweb` archives. New archives can be opened
with 7-Zip. Built-in operating-system extractors do not always support AES-ZIP.

Not included: `.7z` archives, split volumes, Deflate64, solid archives, dictionary
size or thread-count controls, and encrypted file names. Old Python `.tresor`
archives still require the former application.

## Large files and safety

In Direct to disk mode, contents are processed in small blocks and written directly to disk. The file
list uses additional memory; up to 200,000 entries are allowed. Keep enough free
space for the output. FAT32 cannot store individual files of 4 GiB or larger.
Do not modify the source files during processing.

Ordinary file contents and the browser-provided folder structure are preserved,
including empty files and subfolders. Empty-folder-only archives can be created
with encryption set to None. Permissions, links, original timestamps and special
system attributes are not restored. Sensitive system folders may be blocked by
the browser.

**ZIP encryption protects contents, not file names, folder structure or sizes.**
Metadata is not cryptographically authenticated. Changing names or removing whole
entries cannot reliably be detected by the ZIP format alone. Share passwords
separately from archives.

The page embeds zip.js 2.23.0 and its JavaScript Deflate compressor. Network
connections and workers are disabled by policy; no external resources are loaded.
The full MIT and third-party BSD/zlib license notices are embedded in the HTML. The application has not been
independently security-audited and does not replace a backup.

Keep the page open until completion or confirmed cancellation. Normal failures
attempt to clean up new incomplete output. Closing the tab, quitting the browser
or losing power can leave already extracted plaintext files behind.

## Validation

The archive engine and UI have automated coverage. They include actual 7-Zip 26.00 interoperability
for all encryption choices, every listed compression level, both compression
methods, ZIP64 modes and post-save verification. A repetitive fixture compressed
smaller at level 9 than at level 1, confirming the levels are applied. Additional
checks cover password errors, corruption, unsafe paths, existing targets,
cancellation, legacy archives, English UI state and option dependencies.

The earlier Store/AES-256/ZIP64 pipeline also completed a 4 GiB + 1 MiB test with
matching SHA-256 hashes and a successful 7-Zip integrity check. That large test was
not repeated for each new compression level.

UI logic was tested in a simulated environment. Browser policy prevented a live
visual preview and native file-picker testing here. Tests ran on Windows;
macOS and Linux were not practically tested.


## Licensing

The custom ZipNest application code is released under the **MIT License**.
Copyright (c) 2026 NoAuthZone. The complete MIT license text is embedded in
[ZipNest.html](ZipNest.html).

Bundled third-party components retain their own licenses:

- **zip.js 2.23.0:** BSD 3-Clause.
- **zlib-streams-ts:** BSD 3-Clause, with portions under the zlib license.

The complete third-party copyright notices, license conditions and disclaimers
are embedded in the HTML source. Preserve these notices when redistributing the
file. The compressor module export was adapted to a local wrapper for single-file
use; this modification is identified in the embedded notices. Do not imply
endorsement by the library authors.

7-Zip was used for compatibility testing only; no 7-Zip executable is bundled.
The ZipNest name has not been checked for trademark availability.

## Files in this distribution

- **ZipNest.html** — the complete offline application, including its libraries
  and full license notices.
- **README.md** — usage instructions and licensing information.

No build step or package installation is needed. This minimal distribution does
not include the development sources, build scripts or automated tests mentioned
in the validation history above.

## Publishing on GitHub

Upload **ZipNest.html** and **README.md** to the repository root. You can also
attach ZipNest.html to a GitHub Release for a simple download. Users should
download the actual HTML file and open it locally in desktop Chrome or Edge.

Keep all embedded license notices intact. The full license texts are already
included in the distributed HTML; this two-file distribution does not rely on
separate license or notice files.

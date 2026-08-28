# 03. How the updater checks your signature

An Electron app on Windows does not ask a server whether an update is genuine.
It compares the downloaded installer's certificate subject against a list of
publisher names that was baked into the copy of the app already installed on the
user's machine. Everything below was read out of electron-updater 6.8.9 as
installed in node_modules, not from documentation.

## The path through the code

In `out/NsisUpdater.js`, `verifySignature` begins at line 84. Line 87 reads
`publisherName` from `this.configOnDisk`, and line 99 hands it to the verifier
as an array. `configOnDisk` is defined in `out/AppUpdater.js` at line 76 as a
lazy wrapper around `loadUpdateConfig`, which at lines 482 to 486 reads
`this.app.appUpdateConfigPath` off disk and parses it as YAML. The doc comment
at line 157 of the same file names the production location:
`path.join(process.resourcesPath!, "app-update.yml")`.

Read that path again. It is the `app-update.yml` inside the installation the
user already has, not the one inside the update they just downloaded. The list
of acceptable publisher names is frozen at the moment that older build was
packaged.

## The comparison itself

`out/windowsExecutableCodeSignatureVerifier.js` shells out to PowerShell,
runs `Get-AuthenticodeSignature` on the downloaded file, and parses the JSON.
The loop that compares distinguished names begins at line 72. If a configured
publisher name parses as a full DN, every key present in it must match the same
key in the signer's subject. If it parses as a bare string, it is compared only
against the subject's CN, and line 82 logs a warning asking you to configure the
full DN. No match on any entry means the function resolves with a failure string
and line 92 logs that the installer was signed with the wrong certificate.

## The fail-open case

Lines 88 and 89 of `NsisUpdater.js` are the ones worth knowing about: if
`publisherName` is null, `verifySignature` returns null immediately and the
update proceeds. A missing `app-update.yml` is caught the same way. An unsigned
build does not fail the check, it skips it. Convenient while you have no
certificate, a real gap once you distribute installers: verification only
protects users whose installed build carried a publisher name.

There is an escape hatch: `verifyUpdateCodeSignature` has a public setter at
line 25, so the app can replace the verification function wholesale.

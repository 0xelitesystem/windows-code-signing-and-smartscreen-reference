# Windows code signing and SmartScreen reference

A plain-language reference for shipping signed Windows software after the
CA/Browser Forum cut code signing certificate validity to 460 days. It covers
the rule itself, the renewal calendar it forces on you, what a certificate does
and does not do for SmartScreen, and the specific way a certificate change can
silently break in-app updates for Electron apps. Every claim about the
certificate rules is traced to a line in the specification and every claim about
the updater to code read out of an installed package. The SmartScreen behaviour
in chapter 06 is the one part with no primary source to point at, so it is
described in general terms and never quoted.

## The core idea

A 460-day certificate is not a purchase, it is a recurring operational event
whose date moves about 95 days every cycle, and the thing most likely to break
when it comes round is not your build but your users' ability to install the
next update. Shorter validity means more renewals, more renewals mean more
chances for the name in your certificate to change, and on Windows an Electron
app verifies an update against a publisher name list frozen inside the build the
user already has. The failure is silent: the update downloads, verification
fails, nothing installs, and nothing tells you. Timestamping, a bridge release
carrying both old and new names, and a renewal date derived from the actual
certificate are the three things that keep this boring.

## What is inside

- `01-the-460-day-cap.md` - the rule quoted verbatim from section 6.3.2, what
  460 days is in months, and the day count that is not in the specification.
- `02-the-renewal-drift-calendar.md` - the renewal ladder out to 2036 and why
  both an annual and a quarterly reminder are wrong.
- `03-how-the-updater-checks-your-signature.md` - the verification path through
  installed electron-updater 6.8.9, by file, function and line, including the
  fail-open case.
- `04-why-a-subject-name-can-break-updates.md` - the latitude the specification
  gives a CA over your organization name, and the bridge release that survives it.
- `05-timestamps-expiry-and-revocation.md` - what a timestamp buys you at expiry
  and at revocation, and the one OID that takes it away.
- `06-reputation-and-the-policy-oid.md` - what a certificate change does to your
  SmartScreen signal, and why the September 2026 policy OID change does not.

## The stance

Written from the moment before the first certificate purchase, not after. The
author maintains 37 auto-updating Electron applications and, at the time of
writing, none of them is signed: a scan of every build configuration in that
tree found no Windows signing keys at all. That is why this reference is built
out of primary sources rather than war stories. Where a number could not be
confirmed against a file, it was deleted rather than softened, which is why some
things you might expect to find here (certificate prices, issue-tracker
anecdotes, macOS notarization) are absent. Pricing rots and Apple Developer ID
is Apple's own PKI, outside the scope of these requirements.

This reference is independent. It is not affiliated with, endorsed by, or
connected to Microsoft, the CA/Browser Forum, electron-builder, or any
certificate authority.

## Third-party notices

- CA/Browser Forum Baseline Requirements for the Issuance and Management of
  Publicly-Trusted Code Signing Certificates, version 3.11.0. Copyright holder,
  copied verbatim from the document: "Copyright 2026 CA/Browser Forum". Licensed
  by that document under the Creative Commons Attribution 4.0 International
  license. Source: https://github.com/cabforum/code-signing/blob/main/docs/CSBR.md
  Taken: short verbatim quotations of normative sentences, each attributed to
  its section number in the text.
- electron-updater 6.8.9. No code is reproduced here. Behaviour was read from
  the installed package and is described by file, function and line number only.

## Further reading

- https://cabforum.org/working-groups/code-signing/requirements/
- https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/smartscreen-reputation

## Related

- https://github.com/0xelitesystem/semantic-versioning-reference
- https://github.com/0xelitesystem/api-versioning-basics-reference

## Privacy

This repository is plain Markdown. There is no code to run, no build step, no
tracking, no analytics, and no network calls. Reading it sends nothing anywhere.

## License

MIT. Copyright 0xelitesystem 2026.

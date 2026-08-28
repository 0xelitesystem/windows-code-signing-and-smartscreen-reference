# 04. Why a subject name can break updates

The name in your certificate is not a string you control, and the previous
chapter showed the updater comparing it against a list frozen inside builds
users already installed. Together those facts mean a renewal can silently break
in-app updates across your whole install base with no code change at all.

## The CA has documented latitude over your name

The Baseline Requirements version 3.11.0, section 7.1.4.2.3, says the
`subject:organizationName` field must contain the subject's name or DBA as
verified, then grants room to move: the CA may include information differing
slightly from the verified name, such as common variations or abbreviations. Its
own example is a record reading "Company Name Incorporated" that the CA may
render as "Company Name Inc." or "Company Name". Section 7.1.4.2.4 gives EV
certificates the same latitude, rendering the same record "Company Name, Inc.",
and also lets the CA put a DBA at the front of the field followed by the full
legal name in parentheses, or abbreviate a name over 64 characters.

Each is a byte-level change to a string an installed copy of your app compares
exactly. An added comma, a dropped "Inc.", a name trimmed to fit 64 characters:
any one is enough.

## What the mismatch looks like from the user's side

The update downloads. Verification fails. The install does not happen. The user
sees an app that keeps not updating, and the only evidence is a warning line in
a log file they will never open. Nothing crashes, nothing reaches you, and your
release dashboard shows a successful publish.

## Recovery is out-of-band, not impossible

This strands nobody permanently. It silently breaks in-app updates with no
in-app recovery path. A user who downloads the new installer manually gets a
normal NSIS in-place upgrade, and from then on their frozen publisher list is
the new one. The cost is that you have to reach them by some channel other than
the app itself.

## The bridge release

The only way to change a subject safely is to ship the new name before it is
needed. Publish a release whose publisher name list holds both the old and the
new subject, signed with the certificate you still hold, and let it reach the
install base first. Users who take that build accept installers signed under
either name. Users who never take it check the next installer against the old
list alone and refuse it. A new subject only protects users who installed a
build made after you added it.

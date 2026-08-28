# 01. The 460-day cap

A Windows code signing certificate issued on or after 1 March 2026 can be valid
for at most 460 days. That is the whole rule, and it is short enough to quote in
full. From the CA/Browser Forum Baseline Requirements for the Issuance and
Management of Publicly-Trusted Code Signing Certificates, version 3.11.0,
section 6.3.2, verbatim:

> For Code Signing Certificates issued before March 1st, 2026, the validity
> period MUST NOT exceed 39 months. For Code Signing Certificates issued on or
> after March 1st, 2026, the validity period MUST NOT exceed 460 days.

The same document carries a summary row in its Relevant Dates table keyed to
2026-03-01 and pointing at section 6.3.2, with the second sentence repeated
almost word for word. The change arrived with ballot CSC-31, "Maximum Validity
Reduction", listed in the revision table at version 3.10.

## What the numbers actually are

460 days is 15.1 months. It is 95 days longer than a 365-day year, which is
26 percent longer than a year. Against the old ceiling, 39 months works out to
roughly 1,187 days depending on which months it spans, so the new cap is about
2.58 times shorter, not three times shorter. If you buy a certificate every time
the old one lapses, you will do it about 2.6 times as often as before.

One number you will see repeated in secondary write-ups is 1,185 days. Searching
the specification text for the string "1,185" returns zero matches. The
specification says 39 months and never converts it to days. If a summary hands
you a day count for the old cap, that count is somebody's arithmetic, not the
rule.

## Why the cap is the interesting part

Shorter validity is not a cost story, it is a scheduling story. A 39-month
certificate is a thing you buy once and half forget. A 460-day certificate is a
recurring operational event that lands on a different date every time, and the
next chapter is about exactly how it moves.

Scope note: this is the Windows and CA/Browser Forum world. Apple Developer ID
runs on Apple's own PKI and is outside the Baseline Requirements entirely, so
nothing here transfers to a notarized macOS build.

# 05. Timestamps, expiry and revocation

Timestamp your signatures. A timestamp decouples the lifetime of an installer
already sitting on someone's disk from the 460-day lifetime of the certificate
that signed it, which under a short cap is the difference between shipping once
and re-signing your back catalogue.

## What a timestamp is

The Baseline Requirements version 3.11.0 define a Timestamp Authority as a
service that timestamps data using a certificate chained to a public root,
thereby asserting that the data existed at the specified time. It records when
the signature was made, so a verifier can ask whether the certificate was valid
then rather than now. Section 6.3.2 also notes that code signatures may be
verified at any time: download, unpacking, installation, reinstallation,
execution, or a forensic investigation.

## Expiry

The specification's clearest evidence that a timestamped signature survives
certificate expiry is the exception it carves out. Section 7.2 states that if a
code signing certificate contains the Lifetime Signing OID, the code signature
becomes invalid when the certificate expires, even if the code signature is
timestamped, and adds that this OID is for test purposes only. A rule written to
defeat timestamp survival is only needed because survival is the default
elsewhere. The OID is 1.3.6.1.4.1.311.10.3.13, listed in section 7.1.2.3 as one
a certificate may optionally carry. Check it is absent from anything you ship.

Without a timestamp there is no recorded signing time, so a verifier has nothing
to judge against but the certificate's own validity window.

## Revocation

Section 4.9.6 is explicit that revoking a certificate may invalidate the
signatures on all code signed with it, including code that is perfectly sound.
To limit the blast radius the CA may set the moment the certificate is first
considered invalid in the `revocationDate` field of a CRL entry or the
`revocationTime` of an OCSP response, and software should continue to treat
objects timestamped before that date as valid. Again, the timestamp is the
protection. Section 7.2 keeps a revoked serial on the CRL for at least ten years
after expiry.

## The timestamp certificates themselves

Section 6.3.2 caps timestamp certificate validity at 135 months and forbids use
of a timestamp private key more than 15 months after that certificate's
notBefore date. Timestamps anchor to a far longer-lived chain than your own
certificate. That is why they work.

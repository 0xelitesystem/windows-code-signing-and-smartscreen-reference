# 06. What changes your trust signal, and what does not

Changing your certificate does not carry your accumulated SmartScreen publisher
reputation over to the new one, and no certificate, however well established,
gives a brand new binary a head start. The first is routinely overstated, the
second routinely ignored, and what separates them is which signal each is keyed
to.

## Publisher reputation does not carry over

SmartScreen builds a reputation signal around the publisher identity behind a
signature. Move to a different certificate with a different subject and that
accumulated signal does not come with you. Be careful with the word "reset":
the accurate statement is that a certificate change affects the publisher trust
signal, so what you built up does not carry over to the new certificate.

## File reputation starts at zero every time

The part people forget: every new binary you ship is an unknown file. Its
reputation starts at zero regardless of which certificate signed it or how many
downloads its predecessor had. That is why warnings can appear on a release from
a publisher who has signed for years, and it bounds what a certificate change
costs you: only the publisher signal, since the per-file one began at nothing
anyway. Both paragraphs above describe observed behaviour, not specification
text, and are stated without quotation for that reason.

## The policy OID change does not touch this

The Baseline Requirements version 3.11.0 revision table lists version 3.11,
ballot CSC-32, "Mandatory inclusion of a Reserved Policy OID for Subscriber
certificates", effective 15 September 2026. Section 7.1.6.4 states that from
that date a certificate issued to a subscriber must contain exactly one of the
reserved policy OIDs specified in section 7.1.6.1, in the certificate's
CertificatePolicies extension.

Read where it lives: `certificatePolicies`, an X.509 extension, not the subject
distinguished name. The verification path in chapter 03 compares subject DN
fields and nothing else, so a certificate gaining a mandatory policy OID does
not change what an installed app compares against and cannot trigger the update
break in chapter 04. It is a CA-side obligation you can otherwise ignore.

## Independence

This reference is written and maintained independently. It is not affiliated
with, endorsed by, or connected to Microsoft, the CA/Browser Forum, or any
certificate authority. Quotations come from published specification text.

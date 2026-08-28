# 02. The renewal drift calendar

Set an annual calendar reminder for certificate renewal and it will be wrong
every single year, because 460 days is not a year. It is 95 days more than a
365-day year, so each renewal lands about three months later in the calendar
year than an annual anniversary would, and the drift compounds.

## The ladder

Start from the first day the new cap applies and let each certificate run its
full term. The expiry dates are simple date arithmetic, 460 days at a time:

    start     2026-03-01
    cycle 1   2027-06-04   Jun
    cycle 2   2028-09-06   Sep
    cycle 3   2029-12-10   Dec
    cycle 4   2031-03-15   Mar
    cycle 5   2032-06-17   Jun
    cycle 6   2033-09-20   Sep
    cycle 7   2034-12-24   Dec
    cycle 8   2036-03-28   Mar

The month advances by roughly one quarter each time: March, June, September,
December, back to March. It returns to March at cycle 4, not at some distant
horizon, so anyone telling you the month takes dozens of cycles to come round is
wrong.

## The part that actually bites

Returning to the same month is not the same as returning to the same date. Each
pass through March lands about two weeks later than the last: 1 March, then
15 March at cycle 4, then 28 March at cycle 8. One more fortnight of slippage
tips it out of the month entirely, and it does. Cycle 9 lands on 1 July 2037,
and March does not come round again until cycle 27, in 2060. So both mental
models fail, at different speeds. "Renew every year" is wrong by three months at
the first renewal and worsens from there. "Renew on the quarter" survives eight
cycles and then stops describing the schedule at all.

## What to do with that

Do not derive the renewal date from the previous one in your head. Take the
notAfter date printed on the certificate you actually hold, put that exact date
in the calendar, and set the reminder relative to it with enough lead time to
cover CA identity re-verification and hardware token shipping. Then delete the
reminder and re-derive it from the new certificate the moment you receive it.
The only reliable input is the field on the certificate in your hand.

One consequence worth planning for: with a term this short, at least one renewal
will land in a period when you are not shipping. The certificate expiring is not
itself the emergency. What the expiry does to installers you already shipped is
covered in chapter 05.

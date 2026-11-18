# Day 26 â€” Emergency Response

## Learning goals
- Mitigate user impact before pursuing a complete diagnosis.
- Keep response actions bounded, reversible, and within your authority.

## Fundamentals
An incident response reduces harm through clear roles, shared facts, safe mitigation, and verification. A mitigation can restore service before the underlying cause is known; record that uncertainty and investigate afterward. Emergency procedures should be practiced in a controlled environment.

## Practical session (local lab only)
1. Start the localhost server from the [continuation overview](day-16-to-30-follow-on-overview.md) and confirm `/health.txt` returns 200.
2. Record a baseline and timeline in `~/sre-followon-lab/notes/day26.md`.
3. Stop only your own server with Ctrl+C. Observe and record the failed probe.
4. Follow the recovery steps you documented: restart your own server, then verify the health URL and page content.
5. Record mitigation, recovery evidence, remaining uncertainty, and one risky action you deliberately avoided.

Do not test against public, shared, or employer systems. Do not stop an unknown process.

## Check your understanding
- Why can restoring service come before root-cause analysis?
- What evidence confirms mitigation worked?
- Which actions would exceed the boundaries of this drill?

## Done when
You have a short timeline, a safe mitigation record, and a successful post-recovery probe.

## Reading
[Google SRE: Emergency Response](https://sre.google/sre-book/emergency-response/)


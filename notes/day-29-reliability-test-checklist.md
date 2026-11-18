# Day 29 â€” Reliability Test Checklist

## Learning goals
- Test expected healthy and failure behavior in the local lab.
- Track expected versus actual outcomes and recognize test limits.

## Fundamentals
Tests reduce uncertainty about defined behavior; passing a small local test does not prove reliability under concurrency, dependency failures, or realistic load. A useful checklist names preconditions, action, expected result, actual result, and evidence. Keep the test scope safe and repeatable.

## Practical session
1. Create `~/sre-followon-lab/notes/day29-checklist.md`.
2. Check these cases: healthy endpoint (200), missing path (404), your server stopped (connection failure), restored service (200), and version rollback (expected content restored).
3. Record expected and actual result for each case, plus the command or evidence.
4. Fix one weakness in your local probe script or runbook, then repeat the affected case.
5. List limitations, including concurrency, dependencies, and realistic load. Do not generate load.

## Check your understanding
- Which failure is an HTTP response and which is a connection failure?
- What behavior does this checklist not test?
- Can someone repeat each check safely?

## Done when
All five cases have expected/actual results and one documented improvement.

## Reading
[Google SRE: Testing for Reliability](https://sre.google/sre-book/testing-reliability/)


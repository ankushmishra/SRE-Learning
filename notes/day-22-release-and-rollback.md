# Day 22 - Make a change reversible

## Session goal

Make a small local content change, verify both content and health, and restore
the previous version. Write a release checklist that includes a rollback
trigger and exact recovery steps.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

A repeatable release process makes changes easier to understand and reproduce.
A rollback is a planned way to restore a known-good state when verification
fails. A backup alone is not a rollback plan: recovery needs a trigger, a
specific restoration action, and a post-rollback check. This local file exercise
is not a CI/CD pipeline.

## 2. Practical lab

Work only under `~/sre-followon-lab`. Preserve the starting version in your
notes folder:

```bash
cd ~/sre-followon-lab
cp site/index.html notes/index-v1.html
cat site/index.html
```

Change to version 2 and verify the content and health endpoint:

```bash
printf 'version 2\n' > site/index.html
curl -sS --max-time 2 http://127.0.0.1:8000/
curl -sS --max-time 2 -o /dev/null -w '%{http_code}\n' \
  http://127.0.0.1:8000/health.txt
```

Roll back by restoring the saved version, then verify both again:

```bash
cp notes/index-v1.html site/index.html
curl -sS --max-time 2 http://127.0.0.1:8000/
curl -sS --max-time 2 -o /dev/null -w '%{http_code}\n' \
  http://127.0.0.1:8000/health.txt
```

Write a checklist with: pre-check, change, verification, rollback trigger, and
rollback steps. Include a check of user-visible content; a passing health URL
alone does not verify every behavior.

## 3. Completion check and notes

Demonstrate the rollback, not only the saved copy. Why is checking only
`/health.txt` insufficient to verify the changed page?

- Pre-check:
- Change:
- Verification:
- Rollback trigger:
- Rollback steps:
- Result after rollback:

## Done when

- [ ] Version 2 content was served and checked.
- [ ] The previous version was restored and verified.
- [ ] The checklist includes a trigger and executable rollback steps.

## Reading

[Google SRE Book - Release Engineering](https://sre.google/sre-book/release-engineering/): Philosophy, Continuous Build and Deployment, and Configuration Management.


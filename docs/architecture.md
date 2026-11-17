# Architecture

Each stage is a separate module. It takes the output of the stage before it and saves its own results, which means I can rerun or swap out one stage without breaking the others.

Rules:

* Anything that runs target code runs in a throwaway container.
* Every finding has to be reproducible, with the exact steps and environment saved.
* Automation does the grunt work, but I make the decisions.

## Discovery

Collects candidate WordPress plugins from the WordPress.org plugin directory and projects from GitHub, along with basic info like active installs, size and last update date.

## Prioritization

Scores each project. The main factors are how much attack surface it has (network input, file parsing, AJAX and REST endpoints, auth and capability checks), how many active installs it has, whether it's still maintained and has a security policy, and whether similar bugs have shown up in it before.

## Analysis

This is where candidate bugs come from. Static analysis looks for dangerous patterns and data flows, dynamic testing pokes at a running instance, and an LLM helps review the suspicious code paths so I'm not drowning in false positives.

## Lab validation

Each candidate gets deployed in a fresh Docker environment and I try to reproduce it. Only findings with real, demonstrable impact move on.

## Review

I go through the evidence and either approve it, reject it, or send it back for more digging.

## Reporting

The pipeline drafts a technical report for the maintainer, with root cause, impact, affected versions and reproduction steps. It also drafts a public writeup that stays private until disclosure is done.

## Disclosure

Tracks each report from first contact until the fix, the CVE and the public writeup. See [DISCLOSURE.md](../DISCLOSURE.md).

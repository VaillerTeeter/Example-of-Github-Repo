---
description: "Example-of-Github-Repo project overview — multi-language code sample repository. Always apply."
globs: ""
alwaysApply: true
---

# Project Identity

## Project Name & Purpose

Example-of-Github-Repo is a personal code sample repository. It contains independent
code snippets and examples across multiple programming languages, frameworks, and
toolchains — NOT a single application.

## Nature

This is a **collection of loosely-coupled examples**, not a monolithic codebase.
Each language or framework area lives in its own subdirectory (or branch) with
its own build system, package manager, and conventions. Treat each example area
as a separate mini-project when making changes.

## Key Rules

- **No single tech stack.** This repo is language-agnostic. Never assume a
  specific language, framework, or package manager unless you are inside an
  example subdirectory that defines one.
- **All changes go through feature branches.** Never commit or push to `master`.
  See `.clinerules/git-workflow.md` for full git workflow rules.
- **CI is comprehensive.** The lint workflow covers 30+ languages and tools.
  Adding a new example should include appropriate lint configuration and a
  corresponding CI job.
- **No single "build" or "dev" command.** Each example subdirectory may define
  its own instructions.

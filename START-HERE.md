# START HERE

This repo is a project-first course to help you understand frontend systems from CPU to production UI.

## Who This Course Is For
- Frontend engineers who want to go deeper than framework APIs.
- Backend engineers moving into browser/runtime performance work.
- Systems engineers who want practical browser and UI architecture experience.
- Beginners who can code but need a structured path.

## Prerequisite Quick Self-Check
If you answer "yes" to at least 6/10, start directly with a project path.

1. I can write and run small C/C++/Rust or JavaScript programs.
2. I understand loops, functions, recursion, and basic data structures.
3. I can use a terminal, git, and a debugger at a basic level.
4. I know what HTTP requests/responses are.
5. I know what stack and heap mean.
6. I can read docs and source code without needing step-by-step video guidance.
7. I can run small benchmarks and compare results.
8. I can explain async behavior in JavaScript at a basic level.
9. I can work with tests and logs to debug issues.
10. I can commit code changes and open pull requests.

If you scored below 6, use the Beginner Path first.

## Pick Your Learning Path

### Path A: Frontend Dev
Best if you already build React/Next apps.

Order:
1. [Weeks 6-8](weeks/week-6-virtual-dom.md)
2. [docs/what-happens-when-you-type-a-url.md](docs/what-happens-when-you-type-a-url.md)
3. [projects/mini-react](projects/mini-react/README.md)
4. [case-studies/react-rendering-cost.md](case-studies/react-rendering-cost.md)
5. [debugging/perf-debugging.md](debugging/perf-debugging.md)

### Path B: Backend Dev
Best if you know server/network internals.

Order:
1. [Week 1](weeks/week-1-networking-from-scratch.md)
2. [projects/tiny-http-server](projects/tiny-http-server/README.md)
3. [Weeks 2-3](weeks/week-2-3-browser-engine.md)
4. [projects/mini-browser](projects/mini-browser/README.md)

### Path C: Systems Dev
Best if you know low-level programming and want browser/runtime internals.

Order:
1. [Week 0.5](weeks/week-0.5-cpu-memory.md)
2. [Weeks 2-5](weeks/week-2-3-browser-engine.md)
3. [projects/mini-js-runtime](projects/mini-js-runtime/README.md)
4. [docs/browser-code-reading.md](docs/browser-code-reading.md)

### Path D: Beginner
Best if you are comfortable coding but new to systems/browser internals.

Order:
1. [docs/mental-models.md](docs/mental-models.md)
2. [docs/architecture-map.md](docs/architecture-map.md)
3. [Week 0.5](weeks/week-0.5-cpu-memory.md)
4. [Week 1](weeks/week-1-networking-from-scratch.md)
5. [projects/tiny-http-server](projects/tiny-http-server/README.md)

## Study Rhythm (Practical)
- 5 days/week: 2 focused sessions/day, 60-90 minutes each.
- 1 build day/week: only implementation and debugging.
- 1 review day/week: quizzes, checkpoints, and performance notes.

Minimum weekly output:
- 1 project milestone completed.
- 1 debugging playbook run.
- 1 measurement report (before/after).

## How to Use Projects, Experiments, and Debugging Playbooks

### Projects
Projects are the spine of the course. Every major concept maps to an artifact.
- Project briefs: [projects](projects)
- Rubrics define pass/fail and quality bar.

### Experiments
Use experiments to make performance and runtime behavior visible.
- Experiment index: [experiments](experiments)

### Debugging Playbooks
Follow playbooks step-by-step when you hit bottlenecks.
- [Network debugging](debugging/network-debugging.md)
- [Performance debugging](debugging/perf-debugging.md)
- [Memory debugging](debugging/memory-debugging.md)

## How to Track Progress
Use the master tracker:
- [progress/checklist.md](progress/checklist.md)

Also log weekly notes with:
- [templates/weekly-log-template.md](templates/weekly-log-template.md)

## How to Ask for Help
1. Reproduce issue with minimal example.
2. Include expected vs actual behavior.
3. Include trace/log/profiler screenshots or output.
4. Include environment info (OS, compiler/runtime versions).
5. Open a GitHub issue using templates in `.github/ISSUE_TEMPLATE`.

## How to Contribute
- Read [CONTRIBUTING.md](CONTRIBUTING.md)
- Pick a challenge from [challenges](challenges)
- Submit PR with measurements and test evidence

# Audit report template

Save as `plans/reports/ux-audit-<yymmdd-hhmm>-<slug>.md`, or in the project's own reports location.

```markdown
# UX audit: <App name> (<scope>)

Date · Device and OS · Build / how it was run

## Product model
- What / who / primary goal
- Core vs secondary screens
- Most frequent action(s), and where they live

## Flow coverage
| Flow | Entry | States walked | Result (Complete / Fixed / Product decision / Not testable here) | Evidence |

Totals: N flows, N complete, N fixed, N need a decision, N not testable (each with what was tried).

## Flow issues
| # | Issue | Screen | Why it matters | Fix | P |

## UI issues
| # | Issue | Screen / component | Why it looks or feels wrong | Fix | P |

## Consistency issues
| # | Duplicated / inconsistent component | Current behaviour | Canonical behaviour | P |

## Design system
Typography · spacing · colours · radius · shadows · components: what exists, the gaps found, and the tokens added (ideally none).

## Motion
| # | Moment | Problem | Fix (timing / easing / pattern) | P |

## Needs a product decision
Items you did not change because they involve business rules, pricing, data, permissions or scope. Give the options and your recommendation.

## Not changed on purpose
Deliberate choices, with the reason (for example, it follows the approved design).

## Result
- Issues found / fixed / remaining
- What was re-tested on the device (list the flows)
- Typecheck, lint and tests compared with the baseline
- Files touched
- Not verified: device sizes, reduced motion, screen reader, etc.
```

## Reply to the user, in their language

1. **UX audit summary:** found / fixed / remaining, and anything needing a product decision.
2. **Major fixes:** each as *problem → solution → result*.
3. **Remaining risks:** what could not be verified.
4. A link to the report file, and an offer to commit.

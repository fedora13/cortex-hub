# Contributing to Cortex Hub

> Guidelines for contributing code, knowledge, and documentation to the Cortex Hub project.

---

## Development Setup

Fork [lktiep/cortex-hub](https://github.com/lktiep/cortex-hub) on GitHub, then:

```bash
# Clone your fork and keep the original as `upstream`
git clone https://github.com/<your-username>/cortex-hub.git
cd cortex-hub
git remote add upstream https://github.com/lktiep/cortex-hub.git

# Install dependencies (Node >= 22, pnpm >= 10)
corepack enable
pnpm install

# Start development servers
pnpm dev
```

`pnpm install` also installs the [lefthook](../lefthook.yml) git hooks: build, typecheck and lint run
on commit, and the tests run on push.

---

## Branch Strategy

| Branch | Purpose |
|---|---|
| `master` | The only long-lived branch. Every merge builds and publishes images, and CI commits the release bump. |
| `feat/<name>` | New features |
| `fix/<name>` | Bug fixes |
| `docs/<name>` | Documentation updates |

```bash
# Create a feature branch from an up-to-date master
git fetch upstream
git checkout -b feat/add-knowledge-search upstream/master

# After work is complete
git push -u origin feat/add-knowledge-search
# Open a PR against lktiep/cortex-hub → master
```

Because a merge to `master` ships, keep each PR small and single-purpose, and say in the description
how you checked it.

---

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]
```

| Type | Use Case |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation |
| `refactor` | Code restructuring |
| `test` | Adding or updating tests |
| `ci` | CI/CD changes |
| `chore` | Maintenance tasks |

**Examples:**
```
feat(hub-mcp): add code.impact tool with blast radius analysis
fix(dashboard-api): correct SQLite WAL mode initialization
docs(api): add rate limiting documentation
```

---

## Code Standards

- **TypeScript strict mode** — no `any` types without explicit justification
- **ESLint** — zero warnings policy
- **Prettier** — auto-format on save / pre-commit
- **Import from shared packages** — never duplicate shared logic

```typescript
// ✅ Good
import type { KnowledgeItem } from '@cortex/shared-types'
import { formatDate } from '@cortex/shared-utils'

// ❌ Bad — duplicating shared logic
function formatDate(d: Date) { /* ... */ }
```

---

## Pull Request Checklist

- [ ] Branch is up-to-date with `master`
- [ ] Builds: `pnpm build` (the whole workspace, not `--filter`)
- [ ] Types check: `pnpm typecheck`
- [ ] Lint clean: `pnpm lint` (no new errors or warnings)
- [ ] Tests pass for what you changed: `pnpm test`
- [ ] Conventional commit message
- [ ] PR description says what changed, why, and how you checked it
- [ ] Documentation updated (if applicable)
- [ ] No `TODO` or `FIXME` left in code

---

## Knowledge Contributions

Agents can contribute knowledge items automatically. Human reviewers approve contributions weekly:

1. Agent discovers a pattern → calls `knowledge.contribute()`
2. Item is stored with `approved: false`
3. Human reviews in Dashboard → Knowledge screen
4. Approved items become searchable by all agents

### Writing Knowledge Items

Good knowledge items are:
- **Specific** — "Cloudflare Pages requires `NODE_VERSION` env var" not just "deployment issues"
- **Actionable** — include the solution, not just the problem
- **Scoped** — tagged with project and domain
- **Confident** — set confidence ≥ 0.7 only if you're sure

## 1. Scope the run

Settle these before acting. Take them from the prompt; ask only for the target if it is missing.

- **Target**: a URL (web) or an app id / name (mobile). Localhost and staging are the default; for production, get explicit confirmation first, because attacks submit real data.
- **Surface**: the screens or flows to attack. Default: whatever the user named, else the main flow from the landing screen.
- **Attacks**: categories from [references/attacks.md](references/attacks.md). Default: all. The user can pick (`inputs, timing`) or exclude (`-network`).
- **Seed**: an integer for monkey runs. Default: current unix time. Print it so the run can be replayed.
- **Cost**: read the code behind each submit before attacking it. If one starts paid or outward work (model calls, emails, payments), ask the user for a cap, and stub that transport for repeat attacks, as described in [references/web.md](references/web.md). Run against a copy of the app's state when it can be pointed at one.

Done when target, surface, attack list, seed and any cost cap are written at the top of your working notes.

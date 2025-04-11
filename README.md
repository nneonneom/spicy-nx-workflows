# 🌶️ Spicy Nx Workflows

**Spicy Nx Workflows** is a collection of reusable GitHub Actions workflows built to simplify and scale CI/CD pipelines in [Nx](https://nx.dev)-powered monorepos.

These workflows help automate common Nx operations like detecting affected projects, filtering by tags, and running targeted builds/tests — keeping your CI fast and efficient.

---

## 📦 Available Workflows

### `get-affected-projects.yml`

Detects affected Nx projects between the current branch and the `main` branch, optionally filtered by an Nx tag. Outputs a JSON array of project names suitable for matrix jobs.

#### 🔧 Inputs

| Name              | Required | Default | Description |
|-------------------|----------|---------|-------------|
| `nx-project-tag`  | ❌       | _none_  | Optional Nx project tag to filter affected projects (e.g., `build-type:cloudblocks`) |
| `node-version`    | ❌       | `20`    | Node.js version used to install and run dependencies |

#### 📤 Outputs

| Name               | Description |
|--------------------|-------------|
| `affected-projects` | A JSON array of affected project names |

---

## 🚀 Usage Example

```yaml
name: Affected Projects CI

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

jobs:
  get-affected:
    uses: your-org/spicy-nx-workflows/.github/workflows/get-affected-projects.yml@main
    with:
      nx-project-tag: build-type:cloudblocks
      node-version: '20'

  build:
    needs: get-affected
    runs-on: ubuntu-latest
    strategy:
      matrix:
        project: ${{ fromJson(needs.get-affected.outputs.affected-projects) }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install -g pnpm
      - run: pnpm install
      - run: pnpm nx build ${{ matrix.project }}
      - run: pnpm nx test ${{ matrix.project }}
```
## 🧪 Coming Soon

- ✨ Additional reusable workflows for linting, testing, and deployment
- 🏷️ Advanced project filtering (e.g., by type: `app`, `lib`, `infra`)
- ⚡ Dependency caching for faster installs
- 🔁 Automatic detection of changes across multiple base branches
- 📦 Publishable workflow templates for use across multiple repos

## 🙌 Contributions

Contributions are welcome! Feel free to:

- 💬 Open issues for bugs, feature requests, or questions
- 🔧 Submit pull requests to improve or add new workflows
- 📚 Suggest improvements to documentation and examples
- 🌍 Share feedback and use cases to shape future features


# TypeScript Monorepos Course

This repository contains the code and exercises for the TypeScript Monorepos course. The project is a seed catalog application built with Svelte, TypeScript, and Express.

## Prerequisites

- Node.js (version 22.16.0 or higher)
- ppnpm (version 10.0.0 or higher)
- Git

## Getting Started

### 1. Clone the Repository

```bash
git clone git@github.com:mike-north/ts-monorepos-v2.git
cd ts-monorepos-v2
```

### 2. Node Version Management

This project uses Node.js version 22.16.0.

#### Volta

Volta is a great tool for managing node versions across different projects. Get it at [https://volta.sh](volta.sh)

You can install volta in any POSIX-compliant operating system that supports `curl` by running

```sh
curl https://get.volta.sh | bash
```

You may need to close and reopen your terminal before your can verify that your environment has volta installed

```sh
volta --version
> 2.0.2
```

#### Nvm

If you have `nvm` (Node Version Manager) installed, you can automatically use the correct version:

```bash
nvm use
```

### 3. Install `pnpm` if you don't have it already

Make sure you have [`pnpm`](https://pnpm.io/) installed.

If you use `volta` you can just run

```sh
volta install pnpm
```

Alternatively you can follow [`pnpm`'s direct installation instructions](https://pnpm.io/installation)

### 3. Install Dependencies

Install all project dependencies using pnpm:

```bash
pnpm install
```

### 4. Test whether basic tasks work

```sh
pnpm run build      # Build the project
pnpm run lint       # Lint the project
pnpm run test       # Test the project
```

### 5. Test whether the `dev` script works

```sh
pnpm run dev
```

- You should be able to go to http://localhost:3000/api/seeds in a browser and see some JSON
- You should be able to go to http://localhost:5173/ and see a UI that looks like this

## Available Scripts

### Development Scripts

- **`pnpm run dev`** - Start both the server and client in development mode with hot reload

  - Runs the Express server and Vite dev server concurrently
  - Server runs on the backend, client runs on the frontend
  - Uses colored output to distinguish between server (yellow) and client (blue) logs

- **`pnpm run dev-server`** - Start only the Express server in development mode

  - Runs the backend API server using `tsx`

- **`pnpm run dev-client`** - Start only the Vite development server
  - Runs the frontend Svelte application

### Build Scripts

- **`pnpm run build`** - Build the project for production

  - Creates optimized build files in the `dist` directory

- **`pnpm run preview`** - Preview the production build locally
  - Serves the built application for testing

### Testing Scripts

- **`pnpm run test`** - Run tests once (no watch mode)

  - Useful for CI/CD pipelines

- **`pnpm run test:watch`** - Run tests in watch mode

  - Uses Vitest for running tests
  - Automatically re-runs tests when files change

- **`pnpm run test:ui`** - Run tests with Vitest UI

  - Opens a web interface for running and viewing tests

- **`pnpm run test:coverage`** - Run tests with coverage report
  - Generates code coverage reports

### Quality Assurance Scripts

- **`pnpm run check`** - Run TypeScript and Svelte type checking

  - Validates TypeScript types across the project

- **`pnpm run lint`** - Run ESLint to check code quality
  - Checks for code style and potential issues

## Project Structure

```
ts-monorepos-v2/
├── src/
│   ├── server/          # Express server code
│   ├── lib/             # Shared library code
│   ├── models/          # Data models
│   └── utils/           # Utility functions
├── tests/               # Test files
├── public/              # Static assets
├── dist/                # Build output (generated)
└── coverage/            # Test coverage reports (generated)
```

## Tech Stack

- **Frontend**: Svelte 5, TypeScript, Tailwind CSS, DaisyUI
- **Backend**: Express.js, TypeScript
- **Build Tool**: Vite
- **Testing**: Vitest, Testing Library
- **Linting**: ESLint
- **Styling**: Tailwind CSS, PostCSS, Sass

## Getting Started with Development

1. **Start the development environment**:

   ```bash
   pnpm run dev
   ```

2. **Run tests**:

   ```bash
   pnpm run test
   ```

3. **Check code quality**:

   ```bash
   pnpm run lint
   pnpm run check
   ```

4. **Build for production**:
   ```bash
   pnpm run build
   pnpm run preview
   ```

## Course Workflow

This repository is designed to support a hands-on TypeScript monorepos course. Throughout the course, you'll work with:

- TypeScript configuration and compilation
- Monorepo structure and organization
- Shared libraries and dependencies
- Build tools and bundling
- Testing strategies
- Code quality and linting

Happy coding! 🚀

# Tools

## Manypkg

Manypkg is a linter for package.json files

```bash
pnpm add -D @manypkg/cli
pnpm manypkg check
pnpm manypkg fix
```

## Syncpack

Syncpack helps detect and identify variations in external dependency package versions across a project or monorepo, potentially helping to consolidate versions and improve install times.

Reducing version variance can help improve npm/pnpm install times and create a greater sense of the entire repository advancing together.

Syncpack can be integrated into continuous integration (CI) processes to prevent introducing new package version variations and provide actionable failure messages.

```bash
pnpm i syncpack@alpha
pnpm syncpack lint
pnpm syncpack fix
```

## Prettier

Code formatting

## Knip

Knip is a tool that helps us remove unused dependencies and exports from our packages. It's great to detect extraneous dependencies and potentially dead code (or over-exposed code) that TS and eslint don't detect

```bash
pnpm i -D knip
```

## Microsoft API Extractor and API Documenter

### @microsoft/api-extractor

API Extractor analyzes your project—primarily the declaration (`.d.ts`) files in your build output—and generates a rollup.  
It produces a single `.d.ts` file and also creates an API report.

Use it when working with a library-shaped package in a monorepo.

```bash
pnpm i -D @microsoft/api-extractor
pnpm api-extractor init
pnpm api-extractor run --local --verbose
```

### @microsoft/api-documenter

```bash
pnpm i -D @microsoft/api-documenter
pnpm api-documenter markdown -i temp -o docs
```

## Lerna (with nx)

Lerna is a tool for managing **JavaScript/TypeScript monorepos**. Lerna works well with `pnpm` as well.

`nx` cache task artifacts

```bash
pnpm dlx lerna init
pnpm lerna run build
pnpm lerna run lint

# Run the scripts "build", "lint", "test", and "check" across all packages.
# The --stream flag outputs logs from different packages in real-time (interleaved).
pnpm lerna run build,lint,test,check --stream

# Run the same scripts, but limit concurrency to 2.
# This means only 2 packages will run their scripts at the same time.
pnpm lerna run build,lint,test,check --concurrency=2

# Run the scripts only in the package that matches the given scope (@seeds/ui).
# Useful for targeting a specific package instead of all packages.
pnpm lerna run build,lint,test,check --scope=@seeds/ui

# Run the "test" script only in packages that have changed since the "course-progress" git ref (branch, tag, or commit).
# Great for CI/CD optimizations — only tests changed/affected packages.
pnpm lerna run test --since=course-progress

```

### Key Use Cases

#### 1. Versioning Strategy

- **Independent mode**: Each package can have its own version.
- **Fixed/locked mode**: All packages share the same version.
- Handles automatic version bumps based on commit history (when paired with `conventional-commits`).

#### 2. Publishing Workflow

- Automates publishing updated packages to npm.
- Skips unchanged packages.
- Works with both public and private registries.

#### 3. Changelog Generation

- Generates changelogs per package or for the whole monorepo.
- Provides clear release notes based on commits.

#### 4. Orchestrating Commands

- Run scripts across packages (`build`, `test`, `lint`) with filters:
  - By package name.
  - By changed/affected packages since last commit.
- Example: run tests only in changed packages instead of the whole repo.

#### 5. Release Management

- Coordinates multi-package releases in large teams.
- Ensures proper ordering of dependency publishing (e.g., publish a library before apps depending on it).

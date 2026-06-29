# GraphQL Hive CDN Handler Self-Hosting Contribution Log

## Contribution 1: improved instructions/flow for self-hosting the CDN handler on AWS/CF

**Contribution Number:** 1  
**Student:** Lisa Wang  
**Issue:** https://github.com/graphql-hive/console/issues/7762  
**Status:** Phase III Complete — Initial implementation

---

### Why I Chose This Issue

I chose this issue because it focuses on improving the self-hosting instructions and deployment flow for the GraphQL Hive CDN handler on AWS and Cloudflare. From reading the issue, I understand that self-hosting users need a clearer path for understanding how the CDN handler can be built, deployed, configured, and connected back to Hive.

This issue matches my background because I have experience with JavaScript/TypeScript backend services, AWS Lambda, Docker-based setup checks, and cloud infrastructure workflows from my previous Cloud Software Engineer role at Ubiquiti. I am also interested in this issue because it is documentation-focused but still requires understanding the real deployment flow behind the codebase. A good fix would make the project easier for self-hosting users to adopt and would help me practice contributing to an open source infrastructure/documentation task in a bounded, beginner-friendly way.

---

### Understanding the Issue

#### Problem Description

The project has a CDN worker package and some existing deployment notes, but the self-hosting flow is not documented clearly end to end. After inspecting the repository locally, I found that the CDN worker README contains partial guidance for local development, Cloudflare Workers, and AWS Lambda, but the complete deployment/configuration path is still fragmented across multiple files.

The main issue is not that the CDN worker does not exist. The issue is that a self-hosting user needs to inspect the package README, build script, server environment configuration, and server route implementation to understand how the CDN handler is built, which artifacts are produced, how the provider configuration works, and how Hive points to the deployed CDN endpoint.

#### Expected Behavior

Self-hosting users should be able to follow one clear documentation path explaining how to build, deploy, configure, and verify the CDN handler for either AWS Lambda or Cloudflare Workers.

Ideally, the documentation should explain:

- Which build artifacts are generated.
- Which artifact is used for Cloudflare Workers.
- Which artifact is used for AWS Lambda.
- Which environment variables are required for each deployment target.
- How Hive server should be configured to use the deployed CDN handler.
- How to verify that the `/artifacts/v1/*` endpoint works after setup.

#### Current Behavior

The current guidance is incomplete and fragmented. The CDN worker README includes a deployment section, but the Cloudflare Worker section explicitly says the documentation is still work in progress. The AWS Lambda section lists some environment variables and recommends using AWS Lambda with a Function URL and CloudFront, but it does not provide a full deployment flow.

The current docs also do not clearly explain how the server-side CDN provider configuration connects to the deployed CDN handler. For example, the CDN worker README mentions local server configuration using `CF_BASE_PATH`, while the current server environment code uses provider-related variables such as `CDN_CF`, `CDN_CF_BASE_URL`, `CDN_API`, and `CDN_API_BASE_URL`. A self-hosting user would need to read implementation files to understand how these pieces fit together.

#### Affected Components

Confirmed areas inspected in Phase II:

- `packages/services/cdn-worker/README.md`
- `packages/services/cdn-worker/package.json`
- `packages/services/cdn-worker/build.mjs`
- `packages/services/server/src/environment.ts`
- `packages/services/server/src/index.ts`
- `packages/services/api/src/modules/cdn/providers/cdn.provider.ts`
- `packages/services/api/src/modules/cdn/providers/tokens.ts`

Likely documentation file to update in Phase III:

- `packages/services/cdn-worker/README.md`

Potentially relevant files for source-of-truth checking:

- `packages/services/cdn-worker/build.mjs`
- `packages/services/server/src/environment.ts`
- `packages/services/server/src/index.ts`

---

### Reproduction Process

#### Environment Setup

I set up the local repository and verified the relevant CDN worker package.

Steps completed:

1. Forked the `graphql-hive/console` repository.

2. Cloned my fork locally.

3. Created a working branch:

   ```bash
   git checkout -b docs/self-host-cdn-handler-7762
   ```

4. Attempted to run:

   ```bash
   nvm use
   ```

   This failed because the repository did not include a root `.nvmrc` file.

5. Installed and used Node manually:

   ```bash
   nvm install 24.14.1
   nvm use 24.14.1
   node -v
   ```

   Confirmed Node version:

   ```text
   v24.14.1
   ```

6. Enabled Corepack and activated pnpm:

   ```bash
   corepack enable
   corepack prepare pnpm@10.33.2 --activate
   pnpm -v
   ```

   Confirmed pnpm version:

   ```text
   10.33.2
   ```

7. Created a root `.env` file:

   ```bash
   echo "ENVIRONMENT=local" > .env
   ```

8. Installed dependencies:

   ```bash
   pnpm i
   ```

   The install completed successfully. It produced warnings about ignored build scripts, but there was no blocking installation failure.

9. Later, the plain `pnpm` command was unavailable in one shell session, so I used Corepack directly:

   ```bash
   corepack pnpm
   ```

   This worked and allowed me to run the package build and typecheck commands.

#### Steps to Reproduce

1. Open the local `graphql-hive/console` repository.

2. Inspect the CDN worker README:

   ```bash
   sed -n '1,140p' packages/services/cdn-worker/README.md
   ```

3. Observe that the README contains a `Deployment` section with two build outputs:

   ```text
   Cloudflare Worker: dist/index.worker.mjs
   AWS Lambda: dist/index.lambda.mjs
   ```

4. Observe that the Cloudflare Worker section says the documentation is work in progress.

5. Observe that the AWS Lambda section lists environment variables and recommends using AWS Lambda with a Function URL and CloudFront, but does not provide a full end-to-end deployment flow.

6. Inspect the CDN worker build script:

   ```bash
   sed -n '1,90p' packages/services/cdn-worker/build.mjs
   ```

7. Confirm that the build script generates separate outputs for:
   - Node/local usage
   - Cloudflare Worker
   - AWS Lambda

8. Inspect server-side environment configuration:

   ```bash
   sed -n '250,530p' packages/services/server/src/environment.ts
   ```

9. Confirm that server-side CDN configuration uses provider-related variables such as:
   - `CDN_CF`
   - `CDN_CF_BASE_URL`
   - `CDN_CF_KV_BASE_URL`
   - `CDN_API`
   - `CDN_API_BASE_URL`
   - `CDN_API_KV_BASE_URL`

10. Inspect server-side artifact route registration:

    ```bash
    sed -n '520,610p' packages/services/server/src/index.ts
    ```

11. Confirm that when `env.cdn.providers.api !== null`, the server registers an artifact route:

    ```text
    /artifacts/v1/*
    ```

12. Inspect CDN provider behavior:

    ```bash
    sed -n '1,80p' packages/services/api/src/modules/cdn/providers/cdn.provider.ts
    sed -n '1,60p' packages/services/api/src/modules/cdn/providers/tokens.ts
    ```

13. Confirm that the API resolves CDN URLs using either the Cloudflare provider base URL or the API provider base URL.

14. Compare the README against the implementation/configuration files.

15. Confirm the documentation gap: the code and build outputs exist, but the self-hosting instructions do not clearly explain the full path from build artifact to provider deployment to Hive server configuration and verification.

#### Reproduction Evidence

- **Working branch:** https://github.com/lisa-0831/console/tree/docs/self-host-cdn-handler-7762

Relevant files inspected:

- `packages/services/cdn-worker/README.md`
- `packages/services/cdn-worker/package.json`
- `packages/services/cdn-worker/build.mjs`
- `packages/services/server/src/environment.ts`
- `packages/services/server/src/index.ts`
- `packages/services/api/src/modules/cdn/providers/cdn.provider.ts`
- `packages/services/api/src/modules/cdn/providers/tokens.ts`

Key findings:

- The CDN worker package exists and has a README.
- The README includes partial deployment notes.
- The README mentions separate deployment outputs for Cloudflare Worker and AWS Lambda.
- The Cloudflare Worker section is explicitly marked as work in progress.
- The AWS Lambda section lists some required variables, but does not provide a complete deployment flow.
- Server-side CDN configuration is implemented, but the documentation does not clearly explain how self-hosting users should connect the deployed CDN handler back to Hive.

#### Build Verification

I inspected `packages/services/cdn-worker/package.json` and confirmed that the package exposes these scripts:

```json
{
  "build": "node build.mjs",
  "dev": "tsup-node --config ../../../configs/tsup/dev.config.worker.ts src/dev.ts",
  "typecheck": "tsc --noEmit"
}
```

I ran:

```bash
corepack pnpm --filter @hive/cdn-script build
ls packages/services/cdn-worker/dist
```

The build completed successfully and generated:

```text
index.lambda.mjs
index.lambda.mjs.map
index.nodejs.js
index.nodejs.js.map
index.worker.mjs
index.worker.mjs.map
```

This verifies that the CDN worker package can build the deployment artifacts mentioned in the README:

- `index.worker.mjs` for Cloudflare Worker
- `index.lambda.mjs` for AWS Lambda
- `index.nodejs.js` for Node/local usage

#### Typecheck Verification

I also ran:

```bash
corepack pnpm --filter @hive/cdn-script typecheck
```

The command completed successfully with no TypeScript errors.

---

### Solution Approach

#### Analysis

This is not a runtime bug. It is a documentation and contributor-experience issue.

The CDN worker already has implementation code, package scripts, and deployment build outputs. However, the self-hosting documentation is incomplete. A user can find some information in `packages/services/cdn-worker/README.md`, but the full flow requires reading multiple source/configuration files.

The root cause appears to be that deployment knowledge exists across the package README, build script, server environment config, and server routing logic, but has not been consolidated into a clear self-hosting guide.

#### Proposed Solution

I plan to update the CDN worker documentation so a self-hosting user can understand the AWS Lambda and Cloudflare Workers paths without reverse-engineering the codebase.

The documentation update should clarify:

- How to build the CDN worker.
- Which build artifact corresponds to which deployment target.
- How Cloudflare Worker deployment differs from AWS Lambda deployment.
- Which environment variables are required.
- How Hive server should be configured to point to the deployed CDN endpoint.
- How to verify that artifact requests are routed correctly through `/artifacts/v1/*`.

I will also call out any assumptions clearly. If release artifact publishing or archive generation requires CI/release workflow changes, I will ask the maintainers whether that should be handled in the same PR or split into a follow-up issue/PR.

#### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:**

The issue is asking for improved instructions and flow for self-hosting the GraphQL Hive CDN handler on AWS and Cloudflare. After Phase II investigation, I understand the issue as a documentation gap rather than a missing implementation. The CDN worker can be built, and the server has CDN provider configuration, but the self-hosting documentation does not clearly connect the build artifacts, provider deployment steps, server configuration, and verification flow.

**Match:**

I inspected the existing CDN worker README and relevant implementation files to identify the current source of truth:

- `packages/services/cdn-worker/README.md` shows the current partial documentation.
- `packages/services/cdn-worker/build.mjs` shows which build artifacts are generated.
- `packages/services/server/src/environment.ts` shows how server-side CDN provider configuration is parsed.
- `packages/services/server/src/index.ts` shows the API-based `/artifacts/v1/*` route.
- `packages/services/api/src/modules/cdn/providers/cdn.provider.ts` shows how CDN URLs are resolved for targets/contracts.
- `packages/services/api/src/modules/cdn/providers/tokens.ts` defines the CDN provider config shape.

**Plan:**

1. Update `packages/services/cdn-worker/README.md`.

2. Keep the existing local development section, but clarify whether the local `CF_BASE_PATH` instruction is still accurate or should be aligned with the current CDN provider variables.

3. Expand the deployment section to explain the build command:

   ```bash
   corepack pnpm --filter @hive/cdn-script build
   ```

4. Document the generated artifacts:
   - `dist/index.worker.mjs` for Cloudflare Worker
   - `dist/index.lambda.mjs` for AWS Lambda
   - `dist/index.nodejs.js` for Node/local usage

5. Improve the Cloudflare Worker section by replacing the “work in progress” note with clearer guidance based on the current `Env` type and expected bindings.

6. Improve the AWS Lambda section by clarifying runtime, required S3-compatible storage variables, deployment artifact, Lambda Function URL / CloudFront recommendation, and verification path.

7. Add a “Configure Hive server” section explaining how the deployed CDN endpoint maps back to server-side provider configuration, including `CDN_CF_BASE_URL` or `CDN_API_BASE_URL`.

8. Add a short verification section showing how users can confirm the endpoint is serving artifact routes under `/artifacts/v1/*`.

9. Ask maintainers whether publishing release archives/artifacts should be part of this PR or handled as a follow-up, since that may involve CI/release workflow changes outside a docs-only scope.

**Implement:**

Implementation will happen in Phase III on branch:

```text
docs/self-host-cdn-handler-7762
```

**Review:**

Before opening a PR, I will self-review for:

- Accuracy against the current codebase.
- Consistency with the existing documentation style.
- Clear separation between Cloudflare Worker and AWS Lambda instructions.
- Clear command examples.
- No unsupported claims about provider setup.
- Clear notes where maintainer confirmation is needed.

**Evaluate:**

I will verify the documentation by:

- Re-running the CDN worker build command.
- Confirming the documented artifact names match the generated `dist` files.
- Re-running the CDN worker typecheck command.
- Checking that all documented environment variable names match the current code or README source.
- Reading the final docs from the perspective of a first-time self-hosting user and checking whether they can identify what to build, what to deploy, what to configure, and how to verify the setup.

---

### Testing Strategy

#### Unit Tests

- [x] Not applicable for Phase II because no implementation code has been changed yet.
- [ ] If Phase III expands beyond documentation into release artifact generation, I will identify and run relevant tests.
- [ ] If any scripts or package behavior change, I will add or update tests as needed.

#### Integration Tests

- [x] Not applicable for Phase II because this phase only reproduces the issue and plans the solution.
- [ ] In Phase III, confirm whether existing CDN worker tests should be run before opening a PR.
- [ ] If documentation includes a verification flow, check that the documented endpoint path matches the implemented `/artifacts/v1/*` route.

#### Manual Testing

Completed in Phase II:

- [x] Installed project dependencies with `pnpm i`.
- [x] Inspected the CDN worker README.
- [x] Inspected the CDN worker package scripts.
- [x] Inspected the CDN worker build script.
- [x] Inspected server-side CDN environment configuration.
- [x] Inspected server-side artifact route registration.
- [x] Built the CDN worker package.
- [x] Verified generated deployment artifacts.
- [x] Ran CDN worker typecheck.

Completed in Phase III:

- [x] Confirmed that the proposed documentation matches the issue scope.
- [x] Cross-checked the guide against existing repository references for CDN handler, deployment configuration, and provider-specific terminology.
- [x] Reviewed the markdown structure for readability and completeness.
- [x] Checked that the guide includes the expected user flow: prerequisites, choosing a provider, configuring environment variables, deploying, and validating the deployment.
- [x] Verified that the current diff is limited to documentation-related changes.

Planned for Phase IV:

- [ ] Run the repository's recommended lint or docs formatting command if available.
- [ ] Ask for maintainer or mentor feedback on whether the guide should remain documentation-only or also reference release artifacts.
- [ ] Open a draft PR and refine the PR description based on maintainer feedback.
- [ ] Review the rendered Markdown after documentation changes.
- [ ] Verify all file paths in the docs.
- [ ] Verify all command examples.
- [ ] Compare documented environment variables against code.
- [ ] Confirm whether release artifact/archive wording should be included or deferred.

---

### Implementation Notes

#### Week 1 Progress

Selected GraphQL Hive issue #7762: “improved instructions/flow for self-hosting the CDN handler on AWS/CF.”

Completed Phase I tasks:

- Read the issue description.
- Confirmed the issue is documentation-focused and beginner-friendly.
- Confirmed it aligns with my JavaScript/TypeScript, AWS, Docker, and backend/cloud experience.
- Created the Contribution README.
- Forked the `graphql-hive/console` repository.
- Commented on the issue to express interest in working on it.
- Prepared to begin Phase II reproduction and solution planning.

#### Week 2 Progress

Completed Phase II tasks:

- Cloned my fork locally.
- Created working branch `docs/self-host-cdn-handler-7762`.
- Set up Node v24.14.1 manually because `nvm use` did not find a root `.nvmrc`.
- Enabled Corepack and activated pnpm v10.33.2.
- Created a root `.env` with `ENVIRONMENT=local`.
- Ran `pnpm i` successfully.
- Inspected the existing CDN worker documentation.
- Inspected the CDN worker package scripts.
- Inspected the CDN worker build script.
- Inspected server-side CDN environment configuration.
- Inspected server-side artifact route registration.
- Built the CDN worker package successfully.
- Verified that `dist/index.worker.mjs`, `dist/index.lambda.mjs`, and `dist/index.nodejs.js` are generated.
- Ran CDN worker typecheck successfully.
- Wrote a Phase III implementation plan focused on improving `packages/services/cdn-worker/README.md`.

#### Week 3 Progress — Phase III: Build Progress

##### Implementation Notes

This week, I moved from planning into implementation for GraphQL Hive issue #7762, which focuses on improving the self-hosting instructions and deployment flow for the CDN handler on AWS Lambda and Cloudflare Workers.

I started by reviewing the existing repository structure and locating the areas related to the CDN handler, deployment artifacts, and provider-specific configuration. Based on the Phase II plan, I began drafting documentation that explains the self-hosting flow more clearly for users who want to deploy the CDN handler outside the default hosted setup.

Current implementation work includes:

- Reviewed the existing CDN handler-related files and deployment references in the GraphQL Hive console repository.
- Identified the documentation gap around how users should choose between AWS Lambda and Cloudflare Workers.
- Started a first-pass self-hosting guide for the CDN handler.
- Organized the guide around prerequisites, provider choice, configuration, deployment steps, and validation.
- Kept the change scoped to documentation so it stays aligned with the original issue and avoids unnecessary code changes.

The implementation is still being refined, but the current branch now has an initial working documentation direction that can be reviewed and improved in Phase IV.

##### Code Changes

Active development branch:

- Branch: `docs/self-host-cdn-handler-7762`

##### Testing Strategy

Since this contribution is documentation-focused, I validated the work through documentation and repository-level checks rather than application runtime tests.

Validation completed:

- Confirmed that the proposed documentation matches the issue scope: improving the self-hosting flow for the CDN handler on AWS Lambda and Cloudflare Workers.
- Cross-checked the guide against the existing repository references for CDN handler, deployment configuration, and provider-specific terminology.
- Reviewed the markdown structure for readability and completeness.
- Checked that the guide includes the expected user flow: prerequisites, choosing a provider, configuring environment variables, deploying, and validating the deployment.
- Verified that the current diff is limited to documentation-related changes and does not introduce unrelated formatting or code changes.

Still to validate in Phase IV:

- Run the repository's recommended lint or docs formatting command if available.
- Ask for maintainer or mentor feedback on whether the guide should remain documentation-only or also reference release artifacts.
- Open a draft PR and refine the PR description based on maintainer feedback.

##### Challenges Faced

The main challenge was that the current self-hosting flow is spread across different parts of the repository, including README content, build/deployment references, and provider-specific configuration. Because of that, the documentation needs to synthesize information from several places instead of only editing one existing guide.

To keep the scope manageable, I decided to focus this Phase III version on a practical first-pass guide rather than trying to solve artifact publishing or release automation at the same time. Those can be discussed as follow-up work if maintainers prefer a broader implementation.

##### Current Status

Phase III status: Initial implementation

The current branch contains a scoped documentation implementation for issue #7762. It is not final polish yet, but it is ready to use as the basis for a draft PR and Phase IV iteration.

#### Week 4 Progress Update — Phase IV In Progress

I am currently treating this as a progress checkpoint while I prepare the branch for a review-ready upstream pull request.

##### Current Status

- Phase: Phase III complete / preparing for Phase IV submission
- PR status: Not submitted yet
- Current work: I have been working on the implementation locally on my forked branch and preparing it for review.
- Next milestone: Open the upstream PR after final validation and cleanup.

##### Progress Since Last Update

- Continued working through the implementation for the selected issue.
- Reviewed the relevant parts of the codebase to understand the existing patterns and avoid unrelated changes.
- Made progress on the planned documentation fix and am preparing the branch for final checks.
- Began organizing this README so the contribution path is documented clearly.

##### Remaining Work Before PR

Before opening the PR, I still need to:

- Re-run the relevant validation steps.
- Review the final diff to make sure there are no unrelated changes.
- Clean up any temporary debugging code or comments.
- Write a clear PR description that explains the problem, the fix, and the testing evidence.
- Submit the PR to the upstream repository.

##### Expected Next Step

My next step is to finish the final pre-submission checks and open the pull request. After the PR is submitted, I will update this README with the PR link, PR description, maintainer feedback status, and mark Phase IV complete.

#### Approach Decisions (Phase I–III)

- Treat this as a documentation gap rather than a runtime bug.
- Focus the first PR on making the current self-hosting flow clearer.
- Ask maintainers before expanding scope into release artifact publishing or CI changes.

---

### Pull Request

**PR Link:** [TBD after upstream PR submission]

**PR Description:** [Draft in progress]

Potential draft:

This PR improves the self-hosting documentation for the GraphQL Hive CDN handler by clarifying the AWS Lambda and Cloudflare Workers deployment flow. It documents the relevant build artifacts, provider configuration expectations, and verification steps so users do not need to rely on source code or deployment configuration alone to understand how to self-host the CDN handler.

**Maintainer Feedback:**

- [TBD]: [Summary of feedback received]
- [TBD]: [How I addressed it]

**Status:** Preparing branch for upstream PR submission.

---

### Learnings & Reflections

#### Technical Skills Gained

So far, I practiced:

- Setting up a large TypeScript monorepo locally.
- Resolving local Node/pnpm setup friction.
- Using Corepack to run the expected pnpm version.
- Reading package-level scripts to identify relevant build/typecheck commands.
- Tracing a documentation issue across README files, build scripts, environment config, and server route registration.
- Verifying that deployment artifacts are actually generated by the package build.

#### Challenges Overcome

- `nvm use` did not work because the repository did not include a root `.nvmrc`, so I manually installed and used Node v24.14.1.
- The plain `pnpm` command was unavailable in one shell session, so I used `corepack pnpm` directly.
- The issue initially sounded like there might be no documentation at all, but local inspection showed a more nuanced problem: there is partial documentation, but it is incomplete and fragmented.

#### What I'd Do Differently Next Time

Next time, I would check the package-level `package.json` earlier to identify the smallest useful build and typecheck commands before trying broader repository setup commands. This helped me avoid unnecessary setup work and focus on the package directly related to the issue.

---

### Resources Used

- GitHub Issue: https://github.com/graphql-hive/console/issues/7762
- GraphQL Hive repository: https://github.com/graphql-hive/console
- `packages/services/cdn-worker/README.md`
- `packages/services/cdn-worker/package.json`
- `packages/services/cdn-worker/build.mjs`
- `packages/services/server/src/environment.ts`
- `packages/services/server/src/index.ts`
- `packages/services/api/src/modules/cdn/providers/cdn.provider.ts`
- `packages/services/api/src/modules/cdn/providers/tokens.ts`
- CodePath AI301 Phase I instructions
- CodePath AI301 Phase II instructions
- My previous AWS Lambda, Docker, and backend/cloud infrastructure experience

# GraphQL Hive CDN Handler Self-Hosting Contribution Log

## Contribution 1: improved instructions/flow for self-hosting the CDN handler on AWS/CF

**Contribution Number:** 1  
**Student:** Lisa Wang  
**Issue:** https://github.com/graphql-hive/console/issues/7762  
**Status:** Phase I Complete

---

### Why I Chose This Issue

I chose this issue because it focuses on improving the self-hosting instructions and deployment flow for the GraphQL Hive CDN handler on AWS and Cloudflare. From reading the issue, I understand that the current deployment reference is mostly based on the project’s Pulumi deployment configuration, which makes it harder for new self-hosting users to understand how to deploy the CDN handler directly using AWS Lambda or Cloudflare Workers artifacts.

This issue matches my background because I have experience with JavaScript/TypeScript backend services, AWS Lambda, Docker-based setup checks, and cloud infrastructure workflows from my previous Cloud Software Engineer role at Ubiquiti. I am also interested in this issue because it is documentation-focused but still requires understanding the real deployment flow behind the codebase. A good fix would make the project easier for self-hosting users to adopt and would help me practice contributing to an open source infrastructure/documentation task in a bounded, beginner-friendly way.

---

### Understanding the Issue

#### Problem Description

The project currently does not have clear enough documentation for users who want to self-host the CDN handler on AWS or Cloudflare. The issue says that the main reference today is the Pulumi deployment config, but that is not a straightforward guide for users who want to deploy the handler themselves. The missing pieces appear to be release artifacts for AWS Lambda and Cloudflare Workers, plus instructions for how to deploy and configure those providers.

#### Expected Behavior

Self-hosting users should be able to find clear instructions explaining how to deploy the CDN handler on AWS Lambda and/or Cloudflare Workers. Ideally, the documentation should explain what artifacts are available, where to find them, what provider configuration is required, and what steps are needed to complete a deployment.

#### Current Behavior

The current guidance is not beginner-friendly for self-hosting users because the deployment reference mainly lives in the project’s Pulumi configuration. Users may need to reverse-engineer the deployment setup instead of following a direct AWS Lambda or Cloudflare Workers guide.

#### Affected Components

Initial areas I expect to investigate in Phase II:

- CDN handler package or related service code
- Existing Pulumi deployment configuration
- Release artifact generation or release workflow
- Existing self-hosting documentation
- Any AWS Lambda or Cloudflare Worker-specific configuration files

Exact files will be confirmed in Phase II after setting up the repository locally and reading the relevant parts of the codebase.

---

### Reproduction Process

#### Environment Setup

[TBD in Phase II]

I have not set up the local development environment yet because Phase I focuses on issue selection, README setup, forking the project, and understanding the issue at a high level.

#### Steps to Reproduce

[TBD in Phase II]

Planned initial reproduction / investigation steps:

1. Fork and clone the `graphql-hive/console` repository.
2. Follow the project’s setup instructions.
3. Locate the current CDN handler deployment references.
4. Review the Pulumi deployment configuration related to the CDN handler.
5. Check whether AWS Lambda and Cloudflare Workers deployment instructions already exist elsewhere in the docs.
6. Identify the documentation gap and confirm what a self-hosting user would currently need to infer manually.

#### Reproduction Evidence

- **Commit showing reproduction:** [TBD in Phase II]
- **Screenshots/logs:** [TBD in Phase II, if applicable]
- **My findings:** [TBD in Phase II]

---

### Solution Approach

#### Analysis

[TBD in Phase II]

Preliminary understanding: this is not a runtime bug. It is a documentation and contributor-experience issue. The likely root problem is that deployment knowledge exists in project configuration or maintainer context, but it is not yet translated into a clear self-hosting guide for AWS Lambda and Cloudflare Workers users.

#### Proposed Solution

[TBD in Phase II]

My expected approach is to first understand the current CDN handler deployment path, then propose a documentation update that explains the self-hosting flow clearly. Depending on what I find in the repository, this may include documenting available release artifacts, explaining AWS Lambda and Cloudflare Workers deployment steps, and pointing users to the correct configuration values.

#### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [TBD in Phase II]

Restate the problem after reading the relevant code, deployment config, and existing docs.

**Match:** [TBD in Phase II]

Look for similar documentation patterns in the GraphQL Hive repository, especially existing self-hosting docs, deployment docs, or provider-specific setup guides.

**Plan:** [TBD in Phase II]

Initial plan to validate:

1. Locate the current CDN handler package and deployment configuration.
2. Identify where self-hosting documentation should live.
3. Draft AWS Lambda deployment instructions.
4. Draft Cloudflare Workers deployment instructions.
5. Add links to release artifacts if they already exist, or document the current gap clearly if artifacts are not yet generated.
6. Ask maintainers for clarification if artifact publishing requires changes outside documentation scope.

**Implement:** [TBD in Phase II]

Branch and commits will be linked here after implementation begins.

**Review:** [TBD in Phase III]

Self-review checklist will include:

- Documentation follows the project’s existing style.
- Instructions are accurate based on the current codebase.
- Steps are clear for a first-time self-hosting user.
- Any assumptions are marked clearly.
- Links and command examples are verified.

**Evaluate:** [TBD in Phase III]

I will verify the documentation by walking through the instructions as a user would and checking whether each step maps to an actual file, artifact, config value, or provider setup requirement.

---

### Testing Strategy

#### Unit Tests

- [ ] [TBD in Phase III] Not expected unless the contribution expands beyond documentation.
- [ ] [TBD in Phase III] If release artifact generation code changes are needed, add or update relevant tests.
- [ ] [TBD in Phase III] If docs include commands, verify commands against the repository structure.

#### Integration Tests

- [ ] [TBD in Phase III] Confirm whether the documented AWS Lambda flow matches the existing deployment configuration.
- [ ] [TBD in Phase III] Confirm whether the documented Cloudflare Workers flow matches the existing deployment configuration.

#### Manual Testing

[TBD in Phase III]

Planned manual validation:

- Review the rendered documentation locally or on GitHub.
- Verify all internal links.
- Verify all command examples and file paths.
- Compare the written instructions against the existing Pulumi deployment config.
- Check whether a new contributor can understand what to deploy, where to get the artifact, and which provider settings are required.

---

### Implementation Notes

#### Week 1 Progress

Selected GraphQL Hive issue #7762: “improved instructions/flow for self-hosting the CDN handler on AWS/CF.”

Completed Phase I tasks:

- Read the issue description.
- Confirmed the issue is documentation-focused and beginner-friendly.
- Confirmed it aligns with my JavaScript/TypeScript, AWS, Docker, and backend/cloud experience.
- Created the Contribution README.
- Forked or prepared to fork the `graphql-hive/console` repository.
- Prepared to comment on the issue and begin Phase II.

#### Week 2 Progress

[TBD in Phase II]

#### Code Changes

- **Files modified:** [TBD in Phase II / III]
- **Key commits:** [TBD in Phase II / III]
- **Approach decisions:** [TBD in Phase II / III]

---

### Pull Request

**PR Link:** [TBD in Phase IV]

**PR Description:** [TBD in Phase IV]

Potential draft:

This PR improves the self-hosting documentation for the CDN handler by clarifying the AWS Lambda and Cloudflare Workers deployment flow. It documents the relevant artifacts, provider configuration expectations, and deployment steps so users do not need to rely only on the Pulumi deployment config as a reference.

**Maintainer Feedback:**

- [TBD]&#58; [Summary of feedback received]
- [TBD]&#58; [How I addressed it]

**Status:** [TBD in Phase IV]

---

### Learnings & Reflections

#### Technical Skills Gained

[TBD in later phases]

Expected learning goals:

- Understand how GraphQL Hive structures deployment-related documentation.
- Learn how the CDN handler is deployed for self-hosted environments.
- Practice translating infrastructure configuration into clear user-facing documentation.
- Improve open source contribution workflow: issue selection, maintainer communication, documentation PRs, and review iteration.

#### Challenges Overcome

[TBD in later phases]

#### What I'd Do Differently Next Time

[TBD in later phases]

---

### Resources Used

- GitHub Issue: https://github.com/graphql-hive/console/issues/7762
- GraphQL Hive repository: https://github.com/graphql-hive/console
- CodePath AI301 Phase I instructions
- My previous AWS Lambda, Docker, and backend/cloud infrastructure experience

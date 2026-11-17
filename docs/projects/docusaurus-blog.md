# Docusaurus Blog Configuration

## Overview

This document describes how the Docusaurus template was adapted for this project. It covers the main setup steps, configuration changes, and basic usage for the blog site.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Quickstart](#quickstart)
  - [Local Development](#local-development)
  - [Production Build](#production-build)
- [Usage](#usage)
- [Additional Information](#additional-information)
  - [Configuration Changes](#configuration-changes)
    - [Site and repository settings](#site-and-repository-settings)
    - [Navigation and footer](#navigation-and-footer)
    - [Environment example](#environment-example)
    - [Repository README](#repository-readme)
  - [Steps Followed](#steps-followed)
  - [Validation](#validation)

## Prerequisites

Before starting, make sure the following tools are available:

- [Node.js](https://nodejs.org/) (v24 or newer recommended)
- [npm](https://www.npmjs.com/) (comes with Node.js)
- [Git](https://git-scm.com/)
- Optional: [Docker](https://www.docker.com/) for local container-based deployment workflows

## Quickstart

### Local Development

1. Install dependencies:

   ```bash
   npm install
   ```

2. Start the development server:

   ```bash
   npm start
   ```

   This starts the local Docusaurus development server and opens the site in the browser. Most changes are reflected live while editing.

### Production Build

To generate the static site for deployment:

```bash
npm run build
```

This creates the production output in the `build/` folder and is useful for deployment to static hosting platforms.

## Usage

The project is organized as a Docusaurus-based learning journal and blog. Content can be added in the following places:

- `blog/`: blog posts and article metadata
- `docs/`: project documentation and guides
- `src/`: custom React components and page styling
- `static/`: static files such as images or icons

For a local preview, use `npm start`. For deployment, push the relevant branch and use the configured GitHub Actions workflow or follow the deployment guides in the project docs.

## Additional Information

### Configuration Changes

#### Site and repository settings

In `docusaurus.config.ts`, the template's default site title and tagline were replaced with the project-specific branding for this learning journal. The default site URL was changed from the template domain to the repository's GitHub Pages URL.

A `gitRepositoryUrl` variable was added and reads from `GIT_REPOSITORY_URL`. The documentation and blog edit links now point to the project's actual repository instead of the template repository.

#### Navigation and footer

The navbar title and GitHub link were customized for this project. In the footer, the default community links were replaced with project-relevant links, and the copyright text was updated to fit the repository's branding.

#### Environment example

The environment example in `example.env` was updated with the correct base URL, GitHub organization, project name, and repository URL. This ensures the site can be configured consistently across local and deployment environments.

#### Repository README

The root `README.md` was revised to reflect the actual deployment process used by the project. The template-specific deployment instructions and unnecessary sections were removed in favor of the repository's current workflow.

### Steps Followed

1. Updated the environment example with the repository's URL and project configuration.
2. Customized the Docusaurus site metadata, default URL, and edit links.
3. Adjusted the navbar, footer, and branding to match the project identity.
4. Updated the repository documentation to describe the actual deployment setup.
5. Kept the changes focused on the blog setup branch for this project.

### Validation

The project was tested after the configuration changes, and the setup completed successfully.

# Docusaurus Blog Configuration

## Overview

This document describes how the Docusaurus template was adapted for this project. It covers only changes made on the `setup-blog` feature branch compared with `main`.

## Configuration Changes

### Site and repository settings

In `docusaurus.config.ts`, the template's site title and tagline were replaced with the learning journal's title and author description. The default site URL was changed from `https://spmse.github.io` to `https://c-l-a-y-2-1.github.io`.

A `gitRepositoryUrl` variable was added and is read from `GIT_REPOSITORY_URL`. The documentation and blog edit links now use this variable instead of the template repository URL.

### Navigation and footer

The navbar title and GitHub link were changed to identify this project. In the footer, the template's Community links were replaced with a project page link; the GitHub link now points to this repository, and a link to the template repository was added. The copyright text was also replaced with project-specific copyright and attribution.

### Environment example

In `example.env`, the base URL, GitHub organization, and project name were changed to `/my-dso-blog/`, `C-L-A-Y-2-1`, and `my-dso-blog`. The repository URL was added as `GIT_REPOSITORY_URL`.

### Repository README

The root `README.md` was updated to describe deployment through the existing GitHub Actions workflow. The manual `npm run deploy` instructions and the Contributing section were removed.

## Steps Followed

1. Updated the environment example with this repository's base URL, GitHub organization, project name, and repository URL.
2. Customized the Docusaurus site title, tagline, default URL, and edit links.
3. Updated the navbar, footer links, and copyright details to replace the template branding and links.
4. Revised the root README to reflect the existing automated deployment workflow.
5. Made these project changes on the `setup-blog` feature branch.

## Test

I tested the project after applying the configuration changes. The test completed successfully.

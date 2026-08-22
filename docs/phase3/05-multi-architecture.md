# Docker Multi-Architecture

## Overview

During the first deployment test, the Docker image built by GitHub Actions did not match the architecture of node2.

## The Problem

The GitHub Actions runner initially produced:

```text
linux/amd64

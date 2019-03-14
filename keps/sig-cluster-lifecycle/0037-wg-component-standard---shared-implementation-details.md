---
kep-number: 37
title: WG Component Standard - Shared Implementation Details
authors:
  - "@stealthybox"
  - "@luxas"
owning-sig: sig-cluster-lifecycle
participating-sigs:
  - sig-api-machinery
  - sig-cloud-provider
reviewers:
  - "@thockin"
  - "@jbeda"
  - "@bgrant0607"
  - "@smarterclayton"
  - "@liggitt"
  - "@lavalamp"
  - "@andrewsykim"
  - "@cblecker"
approvers:
  - "@thockin"
  - "@jbeda"
  - "@bgrant0607"
  - "@smarterclayton"
editor:
  name: "@stealthybox"
creation-date: 2019-03-13
last-updated: 
status: implementable
---

# WG Component Standard - Shared Implementation Details

## Table of Contents

- [WG Component Standard - Shared Implementation Details](#wg-component-standard---shared-implementation-details)
  - [Table of Contents](#table-of-contents)
  - [Summary](#summary)
  - [Proposal](#proposal)
     - [ComponentConfig](#componentconfig)
     - [CLI](#cli)
     - [HTTPS Serving](#https-serving)
     - [Leader Election](#leader-election)
  - [Risks and Mitigations](#risks-and-mitigations)
  - [Graduation Criteria](#graduation-criteria)
  - [Implementation History](#implementation-history)

# Summary
This KEP serves to fill in the implementation details of [KEP-0032 Create a `k8s.io/component-base` repo](./0032-create-a-k8s-io-component-repo.md).
It specifies desired behavior of k8s components (as defined in KEP-0032) with regard to 
ComponentConfig, kflag, klog, HTTPS, AuthN/AuthZ, kubeconfig, metrics registration, common 
endpoints, and leader election.

# Proposal

## ComponentConfig
The component MUST support ComponentConfig via the `--config` flag
Find a plan for instance- & platform-specific config

Factor out and use shared types from `k8s.io/component-base/config`
Support reading both JSON and YAML
Support reading multiple YAML documents
Support reading config from a file, an HTTPS endpoint, and a folder of `.yaml` and `.json` files
Enforce a strict deserializing codec

## CLI
Share Flag vs config precedence decision and implementation.

Get rid of cobra entirely from server components (if we find something else for docs generation?).
Create a `kflag` package https://github.com/kubernetes/enhancements/pull/764.

Add `--print-config` && `--print-openapi` && `--validate-config` to all components.
Group flags in sections, have a specific section for alpha and deprecated flags.

Use k8s.io/klog for logging in a sane way.

## HTTPS serving
All components should ONLY support HTTPS.
All should support and delegated authentication and authorization, and the default authn/authz kubeconfig should be inclusterconfig.
Some endpoints (e.g. /healthz) can opt out from auth.

Prometheus metrics registration should use its own registry/gatherer (not the global one), so that it is vendorable/extensible.

Common endpoints:
  - `/` (list available endpoints for discovery)
  - `/healthz`
  - `/metrics`
  - `/configz`
  - `/openapi/v2`
  - `/debug/pprof`
  - `/debug/flags`

## Leader Election
Components should leader-elect in a standardized way



# Risks and Mitigations

# Graduation Criteria

# Implementation History

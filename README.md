# ci-workflows

One reusable GitHub Actions workflow, `validate-app-manifests.yaml`. It builds
the kustomize overlays in a repository and checks what comes out: `kubeconform
-strict` against schemas generated from the CRDs your charts actually install,
`promtool` over any `PrometheusRule`, and a name-reference resolver that asks
whether the names one object uses to find another resolve to anything.

That last check is why the file exists. A `configMapGenerator` without
`disableNameSuffixHash` emits `foo-c4bfhb2gmf` while the `GrafanaDashboard`
keeps asking for `foo`; both halves are individually valid, every schema check
passes, and the dashboard renders nowhere. It did that for two and a half
months before anyone noticed.

It is public for a mechanical reason and not an aspirational one: GitHub does
not let a workflow in a public repository call a reusable workflow stored in a
private one, and the repository that needed this most is public. The
alternative was leaving it unguarded.

The workflow's own header comment is the documentation — what each step does,
what is deliberately absent, and what the inputs mean.

## Unsupported

This is a homelab's plumbing, published because publishing it was the cheapest
way to make it reachable. You are welcome to use it and nothing is owed to you
if you do:

- No compatibility promise. Tags exist so you have something immutable to pin —
  pin by tag or SHA, never `@main` — but a new tag may change behaviour in any
  way at any time. Read the diff before moving.
- No issue triage, no support, no release notes.
- It encodes one cluster's opinions about what a valid manifest is.

If any of that is a problem, copy the file. It is one file by design.

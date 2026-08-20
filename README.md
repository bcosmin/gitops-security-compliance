# GitOps Security & Compliance

An enterprise-grade DevSecOps pipeline demonstrating automated security scanning, policy-as-code enforcement, and continuous GitOps delivery using Kubernetes, ArgoCD, Kyverno, and Trivy.

---

## Repository Structure & Architecture

This project implements a **Shift-Left Security** and continuous governance model where security guardrails are embedded directly into the GitOps workflow.

```text
.
├── README.md
├── app
│   └── base
│       ├── deployment.yaml
│       ├── namespace.yaml
│       └── service.yaml
├── argocd
│   ├── application.yaml
│   ├── root-app.yaml
│   └── security-policies-app.yaml
└── policies
    ├── block-latest-tag.yaml
    ├── enforce-non-root.yaml
    ├── kustomization.yaml
    └── require-resources.yaml
```

## Key Tools & Technologies

- ArgoCD (GitOps): Manages the continuous synchronization of security policies and applications from Git to the Kubernetes cluster using an App-of-Apps pattern.
- Kyverno (Policy as Code): Acts as a dynamic Kubernetes admission controller, enforcing security and compliance rules in real-time (`Enforce` mode).
- Kustomize: Aggregates and manages native Kubernetes resource bundles for policies and applications.

## Security Policies (Kyverno)

The project includes three core security policies located in the `policies/` directory:

- `enforce-run-as-non-root`: Prevents container privilege escalation by requiring `securityContext.runAsNonRoot: true.`
- `require-cpu-memory-limits`: Ensures cluster stability by forcing all containers to declare resource requests and limits.
- `block-latest-tag`: Enforces supply chain security and version predictability by rejecting `latest` image tags.

## Workflow & Execution

1. Continuous Governance (GitOps): ArgoCD continuously monitors the repository. Through the `root-app.yaml`, it automatically synchronizes both security policies and base workloads.

2. Admission Control Enforcement: Any incoming deployment attempting to violate security standards (e.g., missing resource limits, running as root, or using the `latest` tag) is automatically rejected by Kyverno at the Kubernetes API server level.

## Getting Started & Installation

1. Deploy via ArgoCD Root Application:
Apply the parent ArgoCD application manifest to your cluster:

    ```bash
    kubectl apply -f argocd/root-app.yaml
    ```

2. Verify Policy Status:
Check if Kyverno has successfully registered the cluster policies:

    ```bash
    kubectl get clusterpolicies
    ```

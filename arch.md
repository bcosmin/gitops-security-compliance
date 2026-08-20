gitops-security-compliance/
├── .github/
│   └── workflows/
│       └── security-ci.yaml         # Pipeline CI: Trivy (IaC & Container scanning) + Validation
├── applications/
│   └── base/
│       ├── namespace.yaml           # Namespace-ul dedicat aplicației
│       ├── deployment.yaml          # Exemplu de aplicație (cu/fără bune practici de securitate)
│       └── service.yaml             # Serviciul Kubernetes pentru acces
├── security-policies/
│   ├── kustomization.yaml           # Kustomization pentru politicile Kyverno
│   ├── enforce-non-root.yaml        # Politică Kyverno: Interzice rularea ca root
│   ├── require-resources.yaml       # Politică Kyverno: Obligă cererile de CPU/Memorie
│   └── block-latest-tag.yaml        # Politică Kyverno: Interzice imagini cu tag-ul 'latest'
├── argocd/
│   ├── root-application.yaml        # ArgoCD App-of-Apps pattern (sau aplicație unică de sync)
│   └── security-policies-app.yaml   # ArgoCD sync pentru politicile de securitate
└── README.md                        # Documentația completă a proiectului
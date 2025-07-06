# 🌐 `vanilla_project_infra`: Your Unified IT Environment Infrastructure Template

This repository serves as the **core Infrastructure as Code (IaC) blueprint** for your Unified IT Environment. It contains the foundational definitions for provisioning and configuring a robust, reproducible, and modular IT infrastructure, designed to support various application projects.

The goal of `vanilla_project_infra` is to provide a "vanilla" or baseline set of infrastructure components (VMs, networking, shared services like Kubernetes, ELK stack, FreeIPA, Ethereum nodes) that can be easily deployed, customized, and managed for different development, testing, or even production-like scenarios.

## 🚀 Purpose

This repository encapsulates the infrastructure logic, allowing for:

- **Reproducibility:** Spin up identical environments consistently.
    
- **Modularity:** Easily include or exclude components based on project needs.
    
- **Version Control:** Track all infrastructure changes like any other codebase.
    
- **Automation:** Integrate with `infractl` to automate deployment and management.
    
- **Scalability:** Define infrastructure that can be scaled up or down as required.
    

## 📁 Repository Structure

```
vanilla_project_infra/
├── terraform/                # Terraform configurations for VM provisioning and cloud resources
│   ├── main.tf               # Main Terraform definitions (VirtualBox VMs, network config)
│   ├── variables.tf          # Input variables for customization
│   ├── outputs.tf            # Outputs from Terraform (e.g., VM IPs, cluster endpoints)
│   └── modules/              # Reusable Terraform modules for specific components
│       ├── kubernetes-cluster/
│       ├── elk-stack/
│       ├── freeipa/
│       └── ethereum-nodes/   # Ethereum node deployment and network setup
├── ansible/                  # Ansible playbooks and roles for configuration management
│   ├── playbooks/            # Main playbooks for service installation and configuration
│   │   ├── deploy_all_services.yml
│   │   └── configure_ethereum.yml
│   ├── roles/                # Reusable Ansible roles
│   └── inventories/          # Dynamic or static Ansible inventories
├── kubernetes/               # Base Kubernetes manifests for shared services
│   ├── metallb/
│   └── nginx-ingress/
├── docker-compose/           # Docker Compose files for any local development services
├── scripts/                  # Any helper scripts specific to infrastructure management
├── README.md                 # This file
└── LICENSE
```

## 🛠️ Usage with `infractl`

This repository is designed to be managed primarily through the `infractl` script, which resides in your `DevAccelerator` vault. Ensure your `Local Development VM` is set up with `devctl` and has access to the necessary infrastructure tools.

**Prerequisites on your `Local Development VM`:**

1. `devctl` script is installed and configured.
    
2. `DevAccelerator` repository is cloned (containing `infractl`).
    
3. VirtualBox is installed.
    
4. Git is installed.
    

**Recommended Workflow:**

1. **Bootstrap Infrastructure Tools:**
    
    ```
    infractl bootstrap infra-tools
    ```
    
    - This command ensures that essential tools like Terraform, Ansible, VirtualBox utilities, cloud CLIs, and Ethereum development tools are installed on your `Local Development VM`.
        
2. **Initialize Infrastructure Repository:**
    
    ```
    infractl init-infra
    ```
    
    - This command clones the `vanilla_project_infra` repository to your `INFRA_REPO_PATH` (default: `~/projects/vanilla_project_infra`) and performs a `terraform init` to download necessary providers and modules.
        
3. **Deploy a New Project's Infrastructure:**
    
    ```
    infractl deploy <project_name>
    ```
    
    - **Example:** `infractl deploy my-web-app`
        
    - This is the core command to provision a new instance of your unified IT environment.
        
    - It executes Terraform configurations (from `vanilla_project_infra/terraform`) to create VMs, set up networking, and deploy core services (Kubernetes, ELK, FreeIPA, Ethereum nodes).
        
    - It then runs Ansible playbooks (from `vanilla_project_infra/ansible`) to configure these services based on the "vanilla" template.
        
    - You can customize deployments by providing project-specific variables (e.g., via `terraform.tfvars` files in a project-specific directory or directly via command-line flags within the `infractl` script).
        
4. **Destroy Project Infrastructure:**
    
    ```
    infractl destroy <project_name>
    ```
    
    - **Example:** `infractl destroy my-web-app`
        
    - Safely tears down all infrastructure components associated with a specific project, ensuring no orphaned resources.
        
5. **Suspend Project VMs (Save Resources):**
    
    ```
    infractl stow <project_name>
    ```
    
    - **Example:** `infractl stow my-web-app`
        
    - Puts all VirtualBox VMs related to the specified project into a saved state, freeing up RAM and CPU on your host machine.
        
6. **Resume Project VMs:**
    
    ```
    infractl unfurl <project_name>
    ```
    
    - **Example:** `infractl unfurl my-web-app`
        
    - Restarts the VirtualBox VMs for the given project from their saved state.
        
7. **Repack/Export Project Artifacts:**
    
    ```
    infractl repack <project_name>
    ```
    
    - **Example:** `infractl repack my-web-app`
        
    - Exports VirtualBox VMs to `.ova` files or bundles project-specific configuration files for archival or portability. The output will be in `~/infractl_repacks/<project_name>`.
        
8. **Backup Project Data/Configurations:**
    
    ```
    infractl backup <project_name>
    ```
    
    - **Example:** `infractl backup my-web-app`
        
    - Triggers backup routines for databases, application data, and critical configuration files associated with the project. Backups are stored in `~/infractl_backups/<project_name>`.
        
9. **Check Project Infrastructure Status:**
    
    ```
    infractl status <project_name>
    ```
    
    - **Example:** `infractl status my-web-app`
        
    - Provides an overview of the deployed infrastructure for a project, including VM states, Kubernetes cluster status, and Terraform state.
10. **Apply Incremental Updates (Conceptual - Future Implementation):**
    
    ```
    # infractl update <project_name>
    ```
    
    - **Note:** This command is currently conceptual. When implemented, it would apply incremental changes to an existing deployed infrastructure without a full tear-down and redeploy. This is useful for minor configuration tweaks or software version updates in a running environment.
        
11. **Scale Infrastructure Components (Conceptual - Future Implementation):**
    
    ```
    # infractl scale <project_name> <component_name> <new_count>
    ```
    
    - **Example:** `# infractl scale my-web-app web-servers 3`
        
    - **Note:** This command is also conceptual. When implemented, it would allow you to scale specific components (e.g., add more web server VMs, increase Kubernetes pod replicas) for a deployed project.     

## 🤝 Contribution & Customization

This `vanilla_project_infra` is designed to be a living template. Feel free to:

- **Fork this repository** to create your own version.
    
- **Add new Terraform modules** for additional services or cloud providers.
    
- **Develop new Ansible roles/playbooks** for specific configurations.
    
- **Extend Kubernetes manifests** for new shared services.
    
- **Refine the `infractl` script** to better suit your workflow and specific infrastructure needs.
    

By maintaining this repository, you build a powerful, reusable foundation for all your future IT projects.
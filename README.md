# Terraform AWS EKS Cluster

Este proyecto implementa un clúster de Kubernetes administrado (EKS) en AWS utilizando Terraform y una arquitectura modular.
El objetivo es aprovisionar de forma reproducible y desacoplada toda la infraestructura necesaria para ejecutar cargas de trabajo sobre Kubernetes en la nube de AWS.

La infraestructura se define siguiendo buenas prácticas de Infrastructure as Code (IaC), separando responsabilidades en módulos reutilizables.

### 📌 ¿Qué hace este proyecto?

Al ejecutar este proyecto, se crean automáticamente los siguientes recursos en AWS:

- Una VPC personalizada, con subredes públicas y privadas.

- Roles y políticas IAM necesarios para el clúster EKS y sus nodos.

- Un clúster EKS completamente funcional.

- Node Groups con configuración de userdata personalizada.

- Secrets en AWS Secrets Manager, pensados para credenciales o configuraciones sensibles.

- Integración completa entre red, seguridad e identidad para Kubernetes.

### Arquitectura del proyecto

<img src="Terraform EKS.drawio.svg" alt="Texto alternativo" width="600">

### 📂 Estructura del proyecto
```
modules/
├── vpc/              # Custom VPC module
│   ├── main.tf       # Definición de VPC, subnets, IGW, NAT, route tables
│   ├── variables.tf  # Variables del módulo VPC
│   └── outputs.tf    # Outputs reutilizables por otros módulos
│
├── iam/              # Custom IAM roles module
│   ├── main.tf       # Roles y políticas IAM para EKS y Node Groups
│   ├── variables.tf
│   └── outputs.tf
│
├── eks/              # Custom EKS cluster module
│   ├── main.tf       # Definición del cluster EKS y node groups
│   ├── variables.tf
│   ├── outputs.tf
│   └── templates/
│       └── userdata.sh  # Script de inicialización para los nodos
│
└── secrets-manager/  # Custom Secrets Manager module
    ├── main.tf       # Creación de secretos en AWS Secrets Manager
    ├── variables.tf
    └── outputs.tf
```
## 🔹 Descripción de los módulos

### VPC

- Crea una red aislada en AWS que incluye:

- Subredes públicas y privadas

- Internet Gateway y NAT Gateway

- Tablas de ruteo

- Configuración lista para EKS

### IAM

- Gestiona los roles y políticas necesarias para:

- El clúster EKS

- Los nodos worker

- Permisos mínimos requeridos por Kubernetes

### EKS

Provisiona:

- El clúster EKS

- Node Groups administrados
  
- Configuración de bootstrap mediante userdata.sh

- Secrets Manager

- Permite almacenar información sensible como:

- Tokens

- Credenciales

- Configuraciones privadas para aplicaciones desplegadas en el clúster

### 🛠️ Tecnologías utilizadas

Las siguientes herramientas deben estar instaladas antes de ejecutar el proyecto:

- Terraform	>= 1.5.0
- AWS CLI	v2.x
- kubectl	>= 1.27
- Amazon EKS	Compatible con la versión del clúster configurada
- Bash	Para scripts de inicialización
  
### ⚙️ Requisitos previos

- Tener una cuenta de AWS activa.

- Configurar credenciales de AWS localmente:

```bash
aws configure
```

Contar con permisos suficientes para crear:

- VPC

- IAM Roles

- EKS

- Secrets Manager

- Tener acceso a una región compatible con EKS.

### 🚀 Cómo desplegar la infraestructura

Desde la raíz del proyecto:

- Inicializar Terraform:

```bash
terraform init
```


- Validar la configuración:

```bash
terraform validate
```

- Visualizar el plan de ejecución:

```bash
terraform plan
```

- Aplicar los cambios:

```bash
terraform apply
```

🔐 Acceso al clúster EKS

- Una vez creado el clúster, configura el contexto de Kubernetes:

```bash
aws eks update-kubeconfig \
  --region <REGION> \
  --name <EKS_CLUSTER_NAME>
```

- Verifica el acceso:

```bash
kubectl get nodes
```

🧹 Eliminación de recursos

Para destruir toda la infraestructura creada:

```bash
terraform destroy
```

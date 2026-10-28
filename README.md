# AWS Terraform Secure Network (Infraestructura como Código)

> **Resumen:** Diseño y despliegue automatizado de una arquitectura de red segura en AWS utilizando Terraform (IaC). El proyecto segmenta el tráfico entre una subred pública y una privada, aplicando el principio de mínimo privilegio mediante Security Groups.

## Contexto
Este repositorio contiene el código y la documentación para aprovisionar una red base en AWS, preparada para alojar una aplicación web que requiere separación de recursos (Frontend en subred pública, Backend/Base de datos en subred privada). Todo el ciclo de vida de la infraestructura es gestionado a través de Terraform.

## Tecnologías Utilizadas
* **Terraform:** Herramienta de Infraestructura como Código (IaC).
* **AWS (Amazon Web Services):** Proveedor de nube (Región us-east-1).
* **Git:** Control de versiones.

## Arquitectura Desplegada
![Diagrama de Arquitectura](docs/Diagrama%20de%20Red.png)

El código `main.tf` crea los siguientes recursos:
* **VPC:** `10.10.0.0/16` (Aislamiento de red).
* **Subred Pública:** `10.10.1.0/24` (Acceso a Internet vía Internet Gateway).
* **Subred Privada:** `10.10.2.0/24` (Sin acceso directo a Internet).
* **Internet Gateway:** Asociado a la VPC para permitir salida a Internet.
* **Tablas de Enrutamiento:** Una pública (ruta `0.0.0.0/0` al IGW) y una privada (aislada).
* **Security Group:** Restringe el tráfico entrante a los puertos TCP 22 (SSH), 80 (HTTP) y 443 (HTTPS).

## Ciclo de Vida del Despliegue
El proyecto documenta el flujo completo de IaC:
1. `terraform init`: Inicialización del entorno y descarga de proveedores.
2. `terraform plan`: Previsualización de los cambios en la infraestructura.
3. `terraform apply`: Creación de los recursos en AWS.
4. `terraform destroy`: Eliminación limpia de los recursos para evitar costos.

## Estructura del Repositorio
* `main.tf`: Código de la infraestructura (VPC, Subredes, SG).
* `docs/`: Evidencias visuales del despliegue (terminal de comandos y consola de AWS).
* `.gitignore`: Protección de archivos de estado y secretos de Terraform.

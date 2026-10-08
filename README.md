# Diseño y Despliegue Automatizado de Red Segura en AWS (Infraestructura como Código - IaC)

## 1. Resumen Ejecutivo y Alcance Operativo
Este repositorio documenta la implementación integral de una arquitectura de red segura en la nube de Amazon Web Services (AWS) para el caso de estudio de FastSolutions SpA. El proyecto ha sido desarrollado en su totalidad mediante Infrastructure as Code (IaC) utilizando Terraform, garantizando la trazabilidad, automatización y estandarización en el ciclo de vida de la infraestructura.

El diseño arquitectónico demuestra competencias avanzadas en el despliegue de redes virtuales aisladas, segmentación rigurosa de tráfico perimetral mediante subredes públicas y privadas, enrutamiento avanzado, control de acceso basado en red y aplicación de principios de seguridad defensiva en entornos cloud.

---

## 2. Topología de la Arquitectura
La infraestructura opera bajo un modelo de alta disponibilidad en la región us-east-1 y se compone de los siguientes elementos lógicos y de red:
- **Virtual Private Cloud (VPC):** Segmento de red principal configurado con un bloque CIDR institucional 10.20.0.0/16, habilitación de soporte DNS y nombres de host.
- **Subredes Públicas:** Diseñadas para recursos orientados al tráfico exterior, interconectadas mediante un Internet Gateway (IGW):
  - `public-subnet-a`: Rango 10.20.1.0/24 en la zona de disponibilidad us-east-1a.
  - `public-subnet-b`: Rango 10.20.2.0/24 en la zona de disponibilidad us-east-1b.
- **Subredes Privadas:** Aisladas de internet para resguardar la capa de persistencia y lógica interna de la aplicación:
  - `private-subnet-a`: Rango 10.20.10.0/24 en la zona de disponibilidad us-east-1a.
  - `private-subnet-b`: Rango 10.20.20.0/24 en la zona de disponibilidad us-east-1b.

---

## 3. Seguridad y Control de Acceso Perimetral
- **Tablas de Enrutamiento Independientes:** Segmentación de rutas que permite asociar de forma explícita el tráfico de salida a internet exclusivamente a las subredes públicas mediante el IGW, manteniendo un aislamiento estricto en las subredes privadas.
- **Security Group (web-app-sg):** Grupo de seguridad perimetral configurado para futuras cargas de trabajo web con reglas estrictas:
  - *Ingress:* Tráfico HTTP (Puerto 80) y HTTPS (Puerto 443) abierto desde cualquier origen controlado (0.0.0.0/0).
  - *Egress:* Salida libre permitida (-1) para la gestión operativa y de actualizaciones de los recursos.

---

## 4. Estructura del Código Terraform (main.tf)
El aprovisionamiento automatizado se rige por bloques de código declarativo estructurados de la siguiente forma:
1. **Configuración del Proveedor:** Declaración del proveedor de AWS sobre la región objetivo us-east-1.
2. **Aprovisionamiento de Red Base:** Creación de la VPC, subredes segregadas y el Internet Gateway (fastsolutions-igw).
3. **Mecanismos de Enrutamiento:** Definición de tablas de rutas públicas y privadas con sus respectivas asociaciones explícitas a cada subred.
4. **Control de Seguridad:** Declaración del grupo de seguridad perimetral con reglas de entrada y salida normalizadas.

---

## 5. Ciclo de Vida y Validación Operativa (IaC)
La validación y el control de calidad del código se ejecutaron bajo los estándares formales del flujo de trabajo de Terraform:
- **terraform init:** Inicialización del directorio de trabajo, descarga de plugins y configuración del backend para el proveedor de AWS.
- **terraform validate:** Comprobación de sintaxis y consistencia estructural del código fuente.
- **terraform plan:** Generación y revisión del plan de ejecución, validando el alta de 13 recursos controlados (VPC, subredes, IGW, tablas de ruteo, asociaciones y grupos de seguridad).
- **terraform apply:** Ejecución exitosa del despliegue automatizado sobre la infraestructura real de AWS.
- **terraform destroy:** Ejecución del protocolo de destrucción controlada, asegurando la remoción total de los recursos para cumplir con la gestión del ciclo de vida.

---

## Evidencias Documentales y Diagramas
- `main.tf`: Código fuente principal de infraestructura como código.
- `diagrama.png`: Esquema visual de la topología de red desplegada en AWS.
- `screenshots.pdf`: Registro gráfico anonimizado de la traza de comandos en terminal (init, plan, apply) y la validación de estados operativos en la consola de administración de AWS.
- `.gitignore`: Archivo de control de versiones configurado para excluir directorios locales, cachés y archivos de estado (*.tfstate) sensibles.

*Nota: Este repositorio representa un estándar de automatización y seguridad replicable para entornos cloud institucionales y operacionales.*

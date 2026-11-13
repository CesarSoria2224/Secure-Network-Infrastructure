# Secure Network Infrastructure — Alcaldía Municipal

**Proyecto académico de diseño y planificación de una infraestructura de red segura**  
**Autor:** Cesar Soria Mita  
**Carrera:** Ingeniería en Tecnologías de la Información y Seguridad — USFX  
**Asignatura:** Seguridad en Redes II | **Informe:** 13 de junio de 2026

## Descripción y alcance

Propuesta de infraestructura para una institución municipal de tres plantas, con aproximadamente **80 usuarios** y crecimiento previsto hasta **120**. El diseño contempla conectividad, segmentación por áreas, seguridad perimetral, acceso remoto, servicios institucionales, virtualización y respaldos.

> **Estado:** diseño académico documentado. No se ha desplegado la infraestructura en producción ni se presentan configuraciones o pruebas operativas como realizadas.

## Evidencias publicadas

- **[Informe completo en PDF](docs/proyecto-alcaldia.pdf)** — requerimientos, arquitectura, equipamiento y presupuesto.
- **[Topología principal](diagrams/topologia-principal.png)**
- **[Segmentación VLAN](diagrams/segmentacion-vlans.png)**
- **[Seguridad perimetral](diagrams/seguridad-perimetral.png)**
- **[Arquitectura DMZ](diagrams/arquitectura-dmz.png)**
- **[Acceso remoto VPN](diagrams/acceso-remoto-vpn.png)**
- **[Infraestructura virtualizada](diagrams/infraestructura-virtualizada.png)**
- **Planos por planta:** [planta baja](diagrams/floor-plans/planta-baja.png) · [primer piso](diagrams/floor-plans/primer-piso.png) · [segundo piso](diagrams/floor-plans/segundo-piso.png)
- **[Notas técnicas y observaciones del diseño](notes/README.md)**

## Decisiones técnicas de la propuesta

| Área | Diseño planteado |
| --- | --- |
| Conectividad | Core switch de capa 3, switches de acceso, MDF/IDF, fibra OM3 y cableado Cat 6/6A |
| Segmentación | VLAN por departamento, servidores, Wi-Fi corporativa, invitados y DMZ; ACL para limitar flujos |
| Perímetro | pfSense como firewall/NAT, con IDS/IPS propuesto mediante Suricata o Snort |
| Servicios expuestos | DMZ para portales y servicios web autorizados |
| Acceso remoto | Alternativas OpenVPN, IPsec o WireGuard, según necesidad |
| Virtualización | Proxmox VE y servicios previstos como AD, DNS, DHCP y archivos |
| Continuidad | NAS, UPS, copias 3-2-1 y definición futura de RPO/RTO |

## Observaciones y mejoras pendientes

El plan de direccionamiento original utiliza `192.168.10.0/24` con VLSM, pero requiere revisión porque prácticamente agota el bloque y no reserva una subred definida para la VLAN 120 (DMZ). En las [notas técnicas](notes/README.md) se presenta **una alternativa de mejora con un bloque /23 y VLAN 130 de administración**, que no debe confundirse con el diseño original entregado.

Una implementación real necesitaría inventario actualizado, matriz de comunicación entre VLAN, configuraciones verificadas, pruebas de seguridad, validación Wi-Fi y ensayos de recuperación.

## Relación con mi perfil de ciberseguridad

Este proyecto evidencia competencias de **diseño seguro de redes, segmentación, mínimo privilegio y defensa perimetral**. Complementa los laboratorios prácticos de detección y monitoreo publicados en [SOC-Home-Lab](https://github.com/CesarSoria2224/SOC-Home-Lab).

**Nota:** los componentes de seguridad son propuestas de diseño; no se afirma que pfSense, IDS/IPS, VPN o Proxmox hayan sido implementados en la institución.

# Notas técnicas de diseño y mejoras propuestas

Estas notas explican las decisiones del proyecto académico de infraestructura segura para una alcaldía municipal. **No son un registro de implementación**. El informe original está disponible en [docs/proyecto-alcaldia.pdf](../docs/proyecto-alcaldia.pdf).

## 1. Requerimientos y arquitectura

La propuesta atiende a unos **80 usuarios**, con crecimiento previsto a **120**, en un edificio de tres plantas. Incluye separación departamental, acceso a servicios, Wi-Fi corporativa y de invitados, acceso remoto, respaldo y continuidad básica.

Se plantean un MDF, IDF por planta, core switch de capa 3, switches de acceso, fibra óptica OM3 entre plantas, Cat 6 horizontal y Cat 6A para puntos de acceso.

## 2. Segmentación y control de acceso

| VLAN | Área o servicio |
| ---: | --- |
| 10 | Atención Ciudadana |
| 20 | Tesorería y Recaudaciones |
| 30 | Catastro |
| 40 | Secretaría, Recursos Humanos y Jurídica |
| 50 | Finanzas |
| 60 | Obras Públicas |
| 70 | Dirección TIC y Sistemas |
| 80 | Alcaldía y Dirección General |
| 90 | Servidores institucionales |
| 100 | Wi-Fi corporativa |
| 110 | Wi-Fi de invitados |
| 120 | DMZ |

El enrutamiento inter-VLAN se propone en el core switch de capa 3, con **ACL de mínimo privilegio**. Ejemplos de políticas: invitados solo a Internet; Atención Ciudadana sin acceso a VLAN financieras; acceso administrativo controlado desde TIC; DMZ sin conexiones arbitrarias hacia la LAN.

### Observación sobre el direccionamiento original

El informe emplea `192.168.10.0/24` y VLSM. Las asignaciones prácticamente consumen ese bloque y no dejan subred definida para la VLAN 120.

**Alternativa posterior, aún no implementada:** utilizar `192.168.10.0/23` y reservar una VLAN 130 de administración.

| VLAN | Red de ejemplo |
| ---: | --- |
| 10 | `192.168.10.0/27` |
| 20 | `192.168.10.32/28` |
| 30 | `192.168.10.48/28` |
| 40 | `192.168.10.64/27` |
| 50 | `192.168.10.96/28` |
| 60 | `192.168.10.112/28` |
| 70 | `192.168.10.128/27` |
| 80 | `192.168.10.160/28` |
| 90 | `192.168.10.176/27` |
| 100 | `192.168.11.0/26` |
| 110 | `192.168.11.64/27` |
| 120 | `192.168.11.96/27` |
| 130 | `192.168.11.128/28` |

Los rangos finales dependen del inventario real y del crecimiento esperado.

## 3. Seguridad perimetral propuesta

- **pfSense:** filtrado, NAT, reglas por zonas, VPN, registros y control de acceso a la DMZ.
- **IDS/IPS:** Suricata o Snort, con necesidad futura de seleccionar reglas, definir interfaces, revisar falsos positivos y validar alertas.
- **DMZ:** portales públicos y proxy inverso; solo puertos publicados desde Internet, administración desde red autorizada y comunicaciones internas excepcionales.
- **VPN:** OpenVPN para usuarios, IPsec para sedes o WireGuard para conexiones administradas, según el caso; se recomienda MFA, certificados y registro de accesos.

No se dispone de configuraciones completas y validadas de estos componentes para la infraestructura municipal.

## 4. Virtualización y continuidad

Se propuso **Proxmox VE** para consolidar Active Directory, DNS, DHCP, archivos, impresión, base de datos, web, monitoreo y respaldos. Un único servidor físico sería un punto único de fallo; convendría evaluar redundancia.

Se contempló un NAS Synology de cuatro bahías con RAID 5. **RAID no sustituye los respaldos**: la propuesta recomienda estrategia 3-2-1, incrementales diarios, completos semanales, copia externa y pruebas de restauración. Deben definirse RPO y RTO. Una UPS facilitaría continuidad breve o apagado controlado.

## 5. Alcance y limitaciones

**Incluido en el proyecto:** análisis de requerimientos, diseño físico/lógico, VLAN, direccionamiento, seguridad perimetral, DMZ, acceso remoto, virtualización, respaldos, equipamiento y presupuesto.

**No realizado como parte de la propuesta:** instalación física, configuración definitiva de pfSense/switches/ACL, despliegue real, pruebas de rendimiento y alta disponibilidad, auditoría en producción, estudio de cobertura Wi-Fi, pruebas de recuperación y actualización de cotizaciones.

## 6. Próximos pasos recomendados

1. Inventario real de usuarios, equipos y aplicaciones.
2. Revisar direccionamiento y definir VLAN de gestión.
3. Elaborar matriz de flujos permitidos entre VLAN.
4. Simular la topología y validar ACL.
5. Evaluar MFA para VPN y reglas de detección.
6. Definir RPO/RTO y ensayar restauraciones.
7. Revisar redundancia, Wi-Fi y presupuesto.

**Criterio de documentación:** usar “se diseñó”, “se propuso” o “se planificó”. Reservar “se implementó” para actividades con configuración, evidencia y resultados reproducibles.

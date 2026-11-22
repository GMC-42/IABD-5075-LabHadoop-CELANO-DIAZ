# Laboratorio Hadoop con Docker Compose

**Alumna** · Gabriela María Celano Díaz  
**Materia** · Big Data Aplicada, módulo 5075

En esta práctica he desplegado un clúster Hadoop de seis servicios para auditar cómo HDFS almacena y replica bloques. Se comprobó la disponibilidad de un fichero durante la caída de un DataNode y ejecución de WordCount con MapReduce sobre YARN.

## Arquitectura del clúster

| Servicio        | Contenedores                          | Función                                                                |
| --------------- | ------------------------------------- | ---------------------------------------------------------------------- |
| NameNode        | `namenode`                            | Gestiona los metadatos de HDFS y registra la ubicación de los bloques. |
| DataNode        | `datanode1`, `datanode2`, `datanode3` | Almacenan los bloques y sus réplicas.                                  |
| ResourceManager | `resourcemanager`                     | Asigna los recursos de YARN a las aplicaciones.                        |
| NodeManager     | `nodemanager`                         | Lanza y supervisa los contenedores de ejecución de YARN.               |

Los seis servicios utilizaron la imagen `apache/hadoop:3` y se comunican por la red `hadoop-net`. El fichero [`docker/config`](docker/config) establece bloques HDFS de **64 MiB**, un factor de replicación de **2** y YARN como entorno de ejecución de MapReduce.

## Arranque y verificación

Con Docker Desktop en ejecución, abrimos PowerShell en la raíz del repositorio y se ejecuto:

```powershell
cd docker
docker compose up -d
docker compose ps
docker compose exec namenode hdfs dfsadmin -report
```

Se comprobó que los seis servicios estuviesen `Up` y que el informe de HDFS mostrase `Live datanodes (3)`.

- [NameNode · Puerto 9870](http://localhost:9870) · En **Datanodes** se comprueba el estado de los tres trabajadores.
- [ResourceManager · Puerto 8088](http://localhost:8088) · En **All Applications** se consulta el estado de WordCount.

## Evidencias e informe

1. [Estado inicial de los tres DataNodes](evidencias/01_namenode_ui_9870.png)
2. [Auditoría de bloques y réplicas en HDFS](evidencias/02_hdfs_fsck_bloques.png)
3. [Simulación de caída de `datanode2`](evidencias/03_simulacion_fallo_datanode2.png)
4. [Ejecución de WordCount en YARN](evidencias/04_yarn_wordcount_8088.png)

[Informe técnico del laboratorio](informe/Celano_Diaz_InformeLabHadoop.pdf)

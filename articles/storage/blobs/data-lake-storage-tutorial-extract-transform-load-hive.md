---
title: 'Tutorial: Extracción, transformación y carga de datos (ETL) mediante Apache Hive en Azure HDInsight'
description: En este tutorial, va a aprender a extraer datos de un conjunto de datos CSV sin procesar, a transformarlos mediante Apache Hive en Azure HDInsight y a cargar los datos transformados en Azure SQL Database mediante Sqoop.
services: storage
author: jamesbak
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jamesbak
ms.openlocfilehash: cfe06720d0afa0f9f5cf22552ba7ab21d4e617c0
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993153"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-apache-hive-on-azure-hdinsight"></a>Tutorial: Extracción, transformación y carga de datos mediante Apache Hive en Azure HDInsight

En este tutorial, realizará una operación de ETL: extracción, transformación y carga de datos. Se toma un archivo de datos CSV sin formato, se importa a un clúster de Azure HDInsight, se transforma con Apache Hive y se carga en una base de datos SQL de Azure con Apache Sqoop.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Extraer y cargar los datos en un clúster de HDInsight.
> * Transformar los datos mediante Apache Hive.
> * Cargar los datos en una instancia de Azure SQL Database mediante Sqoop.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* **Una cuenta de almacenamiento de Azure Data Lake Storage Gen2 configurada para HDInsight**

    Consulte [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2).

* **Un clúster de Hadoop basado en Linux en HDInsight**

    Consulte [Quickstart: Introducción a Apache Hadoop y Apache Hive en Azure HDInsight con Azure Portal](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-linux-create-cluster-get-started-portal).

* **Azure SQL Database**: Use una instancia de Azure SQL Database como almacén de datos de destino. Si aún no tiene ninguna, consulte [Creación de una instancia de Azure SQL Database en Azure Portal](../../sql-database/sql-database-get-started.md).

* **CLI de Azure**: Si no ha instalado la CLI de Azure , consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* **Un cliente de Secure Shell (SSH)**: Para más información, consulte [Conexión a HDInsight (Apache Hadoop) mediante SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]
> Los pasos descritos en este artículo requieren un clúster de HDInsight que use Linux. Linux es el único sistema operativo que se usa en la versión 3.4 de Azure HDInsight o en versiones posteriores. Consulte la información sobre la [retirada de HDInsight en Windows](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="download-the-flight-data"></a>Descarga de los datos de vuelo

Este tutorial se utilizan datos de vuelo de Bureau of Transportation Statistics para demostrar cómo realizar una operación de ETL. Debe descargar estos datos para completar el tutorial.

1. Vaya a [Research and Innovative Technology Administration, Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

1. En la página, seleccione los siguientes valores:

   | NOMBRE | Valor |
   | --- | --- |
   | **Filter Period** |January |
   | **Fields** |FlightDate, OriginCityName, WeatherDelay |

1. Borre el resto de los campos.

1. Seleccione **Descargar**. Obtendrá un archivo .zip con los campos de datos que ha seleccionado.

## <a name="extract-and-upload-the-data"></a>Extracción y carga de datos

En esta sección, se cargarán datos en el clúster de HDInsight. 

1. Abra una ventana del símbolo del sistema y use el siguiente comando de copia segura (Scp) para cargar el archivo .zip en el nodo principal del clúster de HDInsight:

   ```bash
   scp <file-name>.zip <ssh-user-name>@<cluster-name>-ssh.azurehdinsight.net:<file-name.zip>
   ```

   * Reemplace el marcador de posición `<file-name>` por el nombre del archivo .zip.
   * Reemplace el marcador de posición `<ssh-user-name>` por el inicio de sesión SSH para el clúster de HDInsight.
   * Reemplace el marcador de posición `<cluster-name>` por el nombre del clúster de HDInsight.

   Si usa una contraseña para autenticar el inicio de sesión SSH, se le pedirá la contraseña.

   Si utiliza una clave pública, es posible que tenga que usar el parámetro `-i` y especificar la ruta de acceso a la clave privada correspondiente. Por ejemplo, `scp -i ~/.ssh/id_rsa <file_name>.zip <user-name>@<cluster-name>-ssh.azurehdinsight.net:`.

2. Cuando la carga haya finalizado, conéctese al clúster mediante SSH. En el símbolo del sistema escriba el siguiente comando:

   ```bash
   ssh <ssh-user-name>@<cluster-name>-ssh.azurehdinsight.net
   ```

3. Utilice el comando siguiente para descomprimir el archivo .zip:

   ```bash
   unzip <file-name>.zip
   ```

   El comando extrae un archivo **.csv**.

4. Use el comando siguiente para crear el sistema de archivos de Data Lake Storage Gen2.

   ```bash
   hadoop fs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/
   ```

   Reemplace el marcador de posición `<file-system-name>` por el nombre que desee asignar al sistema de archivos.

   Reemplace el marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

5. Utilice el comando siguiente para crear un directorio.

   ```bash
   hdfs dfs -mkdir -p abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data
   ```

6. Use el comando siguiente para copiar el archivo *.csv* en el directorio:

   ```bash
   hdfs dfs -put "<file-name>.csv" abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data/
   ```

   Escriba el nombre del archivo entre comillas si contiene espacios o caracteres especiales.

## <a name="transform-the-data"></a>Transformar los datos

En esta sección se usa Beeline para ejecutar un trabajo de Apache Hive.

Como parte del trabajo de Apache Hive importe los datos del archivo .csv en una tabla de Apache Hive denominada **delays**.

1. En la ventana del símbolo del sistema de SSH que ya tiene para el clúster de HDInsight, use el siguiente comando para crear y editar un nuevo archivo denominado **flightdelays.hql**:

   ```bash
   nano flightdelays.hql
   ```

2. Modifique el siguiente texto reemplazando los marcadores de posición `<file-system-name>` y `<storage-account-name>` por el nombre del sistema de archivos y de la cuenta de almacenamiento. A continuación, copie y pegue el texto en la consola nano pulsando la tecla MAYÚS a la vez que hace clic con el botón derecho del ratón.

   ```hiveql
   DROP TABLE delays_raw;
    CREATE EXTERNAL TABLE delays_raw (
       FL_DATE string,
       ORIGIN_CITY_NAME string,
       WEATHER_DELAY float)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION 'abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data';
   DROP TABLE delays;
   CREATE TABLE delays
   LOCATION 'abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/processed'
   AS
   SELECT FL_DATE AS FlightDate,
       substring(ORIGIN_CITY_NAME, 2) AS OriginCityName,
       WEATHER_DELAY AS WeatherDelay
   FROM delays_raw;
   ```

3. Guarde el archivo mediante CTRL + X y, a continuación, escriba `Y` cuando se le solicite.

4. Para iniciar Hive y ejecutar el archivo **flightdelays.hql**, use el siguiente comando:

   ```bash
   beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
   ```

5. Cuando finalice el script __flightdelays.hql__, use el siguiente comando para abrir una sesión interactiva de Beeline:

   ```bash
   beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
   ```

6. Cuando aparezca `jdbc:hive2://localhost:10001/>` en el símbolo del sistema, utilice la consulta siguiente para recuperar los datos importados sobre los retrasos de vuelos:

   ```hiveql
   INSERT OVERWRITE DIRECTORY 'abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output'
   ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
   SELECT regexp_replace(OriginCityName, '''', ''),
       avg(WeatherDelay)
   FROM delays
   WHERE WeatherDelay IS NOT NULL
   GROUP BY OriginCityName;
   ```

   Esta consulta recupera una lista de ciudades que experimentaron demoras por inclemencias del tiempo, junto con el tiempo medio de retraso, y la guarda en `abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output`. Más adelante, Sqoop leerá los datos desde esta ubicación y los exportará a Azure SQL Database.

7. Para salir de Beeline, escriba `!quit` en el símbolo del sistema.

## <a name="create-a-sql-database-table"></a>Creación de una tabla de SQL Database

Necesita el nombre del servidor de la base de datos SQL para esta operación. Complete estos pasos para buscar el nombre del servidor.

1. Vaya a [Azure Portal](https://portal.azure.com).

2. Seleccione **Bases de datos SQL**.

3. Filtre por el nombre de la base de datos que ha elegido usar. El nombre del servidor aparece en la columna **Nombre de servidor**.

4. Filtre por el nombre de la base de datos que desea usar. El nombre del servidor aparece en la columna **Nombre de servidor**.

    ![Obtener detalles del servidor de Azure SQL Server](./media/data-lake-storage-tutorial-extract-transform-load-hive/get-azure-sql-server-details.png "Get Azure SQL server details")

    Hay muchas maneras de conectarse a SQL Database y crear una tabla. En los siguientes pasos se utiliza [FreeTDS](http://www.freetds.org/) desde el clúster de HDInsight.

5. Para instalar FreeTDS, use el siguiente comando desde una conexión SSH al clúster:

   ```bash
   sudo apt-get --assume-yes install freetds-dev freetds-bin
   ```

6. Cuando finalice la instalación, use el comando siguiente para conectarse al servidor de SQL Database.

   ```bash
   TDSVER=8.0 tsql -H <server-name>.database.windows.net -U <admin-login> -p 1433 -D <database-name>
    ```
   * Reemplace el marcador de posición `<server-name>` por el nombre del servidor de SQL Database.

   * Reemplace el marcador de posición `<admin-login>` por el inicio de sesión de administrador para SQL Database.

   * Reemplace el marcador de posición `<database-name>` por el nombre de la base de datos.

   Cuando se le solicite, escriba la contraseña para el inicio de sesión del administrador de SQL Database.

   Recibirá una salida similar al texto siguiente:

   ```
   locale is "en_US.UTF-8"
   locale charset is "UTF-8"
   using default charset "UTF-8"
   Default database being set to sqooptest
   1>
   ```

7. En el símbolo del sistema `1>`, escriba las siguientes instrucciones:

   ```hiveql
   CREATE TABLE [dbo].[delays](
   [OriginCityName] [nvarchar](50) NOT NULL,
   [WeatherDelay] float,
   CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED
   ([OriginCityName] ASC))
   GO
   ```

8. Cuando se haya especificado la instrucción `GO`, se evaluarán las instrucciones anteriores.

   La consulta crea una tabla denominada **delays** con un índice agrupado.

9. Use la siguiente consulta para comprobar que se ha creado la tabla:

   ```hiveql
   SELECT * FROM information_schema.tables
   GO
   ```

   La salida será similar al siguiente texto:

   ```
   TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
   databaseName       dbo             delays        BASE TABLE
   ```

10. Entrar `exit` at the `1>` .

## <a name="export-and-load-the-data"></a>Exportación y carga de los datos

En las secciones anteriores, ha copiado los datos transformados en la ubicación `abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output`. En esta sección, va a usar Sqoop para exportar los datos de `abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output` a la tabla que ha creado en la instancia de Azure SQL Database.

1. Utilice el comando siguiente para comprobar si Sqoop puede ver la instancia de SQL Database:

   ```bash
   sqoop list-databases --connect jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433 --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD>
   ```

   El comando devuelve una lista de bases de datos, incluida la base de datos en la que anteriormente ha creado la tabla **delays**.

2. Use el comando siguiente para exportar datos de la table **hivesampletable** a la tabla **delays**:

   ```bash
   sqoop export --connect 'jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433;database=<DATABASE_NAME>' --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD> --table 'delays' --export-dir 'abfs://<file-system-name>@.dfs.core.windows.net/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
   ```

   Sqoop se conecta a la base de datos que contiene la tabla **delays** y exporta los datos del directorio `/tutorials/flightdelays/output` a dicha tabla.

3. Cuando finalice el comando `sqoop`, use la utilidad tsql para conectarse a la base de datos:

   ```bash
   TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -P <ADMIN_PASSWORD> -p 1433 -D <DATABASE_NAME>
   ```

4. Use las instrucciones siguientes para comprobar que los datos se exportaron a la tabla **delays**:

   ```sql
   SELECT * FROM delays
   GO
   ```

   Debería ver una lista de los datos de la tabla. La tabla incluye el nombre de la ciudad y el tiempo medio de retraso de los vuelos promedio a la ciudad.

5. Escriba `exit` para salir de la utilidad tsql.

## <a name="clean-up-resources"></a>Limpieza de recursos

Todos los recursos utilizados en este tutorial son preexistentes. No es necesaria ninguna limpieza.

## <a name="next-steps"></a>Pasos siguientes

Para conocer otras formas de trabajar con datos en HDInsight, consulte el artículo siguiente:

> [!div class="nextstepaction"]
> [Extracción, transformación y carga de datos mediante Azure Databricks](./data-lake-storage-use-hdi-cluster.md)

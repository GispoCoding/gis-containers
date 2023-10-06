# GIS containers

This repository provides a Docker Compose configuration to quickly set up PostgreSQL(PostGIS) and GeoServer for testing purposes. With this setup, you can easily spin up these services in containers, making it convenient for testing and development.

## Prerequisites

To use this repository, you must have Podman and Podman-Compose or alternatively Docker and Docker-Compose installed on your system.

To install Podman follow the instructions [here](https://podman.io/getting-started/installation).
For podman-compose you need Python on your system. To install Python follow the instructions [here](https://www.python.org/downloads/).
```bash
pip install --user podman-compose
```

To install Docker and Docker-compose follow the instructions [here](https://docs.docker.com/get-docker/).

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
```

### 2. Configure the environment variables
To configure the environment variables for your PostgreSQL and GeoServer containers, follow these steps:

1. Locate the `.env.example` file in the root of this repository.
4. Copy the `.env.example` and rename it to `.env`:

    ```bash
    cp .env.example .env
    ```

5. Open the .env file using a text editor of your choice.
6. Modify the environment variables according to your requirements.  
Here's a brief explanation of the variables:

    ```
    GEOSERVER_VERSION: GeoServer version to use.
    GEOSERVER_EXTENSIONS: List of GeoServer extensions to install (if any). See list of available extensions in 
    GEOSERVER_SKIP_DEMO_DATA: Set to true to skip loading GeoServer demo data, or false to load it.
    GEOSERVER_PORT: Port on which GeoServer will run.

    POSTGRES_VERSION: PostgreSQL version to use.
    POSTGIS_VERSION: Version of PostGIS extension for PostgreSQL.
    POSTGRES_PASSWORD: Password for the postgres user.
    POSTGIS_PORT: Port on which PostgreSQL will run.
    ```

7. Save the .env file with your changes.

Now, when you start the PostgreSQL and GeoServer containers using Docker Compose as described in the next section, the containers will use the configuration from the .env file you've just created or modified.


### 3. Start the containers

This command will download the required Docker images (if not already downloaded) and start the containers in the background.
```bash
docker-compose up -d
```

>**Hint:**  
>To start only the desired containers, you can specify the container names as arguments to the `docker compose up` command.  
For example, to start only the PostGIS container, you can run the following command: `docker compose up -d postgis`.  
Alternatively to start only the geoserver container, you can run the following command: `docker compose up -d geoserver`.

After starting the containers, you can access the GeoServer web interface at http://localhost:8080/geoserver. As the ports use the one you specified in the `.env` file.  
PostgreSQL database can be accessed with the following parameters:
- Host: 'localhost'
- Port: the one specified in `.env`
- Database: 'postgres'
- Username: 'postgres'
- Password: the one specified in `.env`


To stop the containers, run the following command:
```bash
docker compose down
```

## Geoserver

Default credentials are:
- username: admin
- password: geoserver

### Data directory

The data directory is located in the `./geoserver/data` directory. This directory is mounted as a volume in the geoserver container. This means that any changes you make to the data directory will be reflected in the container and vice versa.

#### Directory for local files
You can copy your local files to the `./geoserver/data/data` directory.

### Extensions

You can install GeoServer extensions by adding them to the `GEOSERVER_EXTENSIONS` variable in the `.env` file. The extensions will be installed when you start the container. This will download the extensions from the GeoServer repository and install them in the container.

**Available extensions:**
```
app-schema   gdal            jp2k          ogr-wps          web-resource
authkey      geofence        libjpeg-turbo oracle           wmts-multi-dimensional
cas          geofence-server mapml         params-extractor wps-cluster-hazelcast
charts       geopkg-output   mbstyle       printing         wps-cluster-hazelcast
control-flow grib            mongodb       pyramid          wps-download
css          gwc-s3          monitor       querylayer       wps-jdbc
csw          h2              mysql         sldservice       wps
db2          imagemap        netcdf-out    sqlserver        xslt
dxf          importer        netcdf        vectortiles      ysld
excel        inspire         ogr-wfs       wcs2_0-eo
```

Alternatively you can copy the zip files of the extensions to the `./geoserver/additional_libs` directory. This directory is mounted as a volume in the geoserver container. The extensions will be installed when you start the container.

### Fonts
To add fonts to the geoserver container, copy the font files to the `./geoserver/fonts` directory. This directory is mounted as a volume in the geoserver container. The fonts will be available in the geoserver container when you start the container.

## PostGIS

The data directory of the PostgreSQL database is mounted as a named container volume. This means that changes to database are persisted even after the container is stopped or removed.

To start with a clean database, you can delete the container and remove the named volume using the following command:
```bash
docker compose down -v postgis
```

# Java 8 Library Package

## 1. Overview

This package contains precompiled third-party Java libraries (JAR files) and their bundled dependencies for persistence APIs, database connectivity, geospatial processing, general-purpose utilities, and Redis integration.

The libraries were selected for Java 8 compatibility and tested individually on a Windows 64-bit machine using a portable Java 8 runtime. The package is intended for manual classpath-based integration, without requiring Maven or Gradle to download the included dependencies.

| Property | Details |
|---|---|
| Target Java version | Java 8 (1.8) |
| Tested Java version | 1.8.0_512 |
| Tested platform | Windows, AMD64 (64-bit) |
| Library profiles | 11 |
| Test result | All 11 individual smoke tests passed |
| Original target update | 1.8.0_92 (not directly tested) |
| Java runtime included | No; use an existing or portable Java 8 runtime |

## 2. Package structure

Each subfolder under `lib/` is a separate library profile containing the main JAR and its bundled dependencies. Preserve this structure when extracting or transferring the package.

```text
java8-libraries.zip
├── README.md
└── lib/
    ├── jakarta/
    ├── javax/
    ├── postgresql-42.2.18/
    ├── postgresql-42.7.1/
    ├── geohash/
    ├── geotools/
    ├── guava/
    ├── h3/
    ├── jedis/
    ├── spring-redis/
    └── jts/
```

The GeoTools profile contains an extracted binary distribution and may have nested directories. JAR files are ZIP-based archives; keep them in their original `.jar` format. The original GeoTools ZIP is not required at runtime if its contents have already been extracted.

## 3. Library inventory and purpose

| Profile | Main JAR | Purpose |
|---|---|---|
| `jakarta` | `jakarta.persistence-api-3.0.0.jar` | Jakarta Persistence API (JPA): entity annotations and ORM interfaces under `jakarta.persistence`. |
| `javax` | `javax.persistence-api-2.2.jar` | Java Persistence API 2.2 for legacy Java EE applications using `javax.persistence`. |
| `postgresql-42.2.18` | `postgresql-42.2.18.jar` | PostgreSQL JDBC driver for SQL queries, prepared statements and transactions. Older version retained for compatibility. |
| `postgresql-42.7.1` | `postgresql-42.7.1.jar` | Newer PostgreSQL JDBC driver for the same database access operations. |
| `geohash` | `geohash-1.4.0.jar` | Encodes latitude/longitude into compact GeoHash strings for spatial indexing and proximity searches. |
| `geotools` | `gt-referencing-28.6.1.jar` and other GeoTools 28.6.1 modules | GIS toolkit for coordinate reference systems, coordinate transformations, spatial data formats and related operations. |
| `guava` | `guava-32.1.3-jre.jar` | General Java utilities: collections, caching, hashing, strings and concurrency helpers. |
| `h3` | `h3-4.3.1.jar` | Uber H3 hierarchical spatial indexing: maps coordinates to geographic grid cells and supports neighborhood queries. |
| `jedis` | `jedis-3.8.0.jar` | Redis Java client for keys, values, hashes, lists, sets, expiration and other Redis commands. |
| `spring-redis` | `spring-boot-starter-data-redis-2.7.18.jar` | Spring Boot Redis integration, including Spring Data Redis, RedisTemplate, Lettuce and Jedis client support. |
| `jts` | `jts-core-1.19.0.jar` | Geometry processing: points, lines, polygons, intersections, buffering, spatial predicates and WKT parsing. |

### API and artifact selection notes

- Jakarta Persistence 3.0.0 is the selected replacement for the originally requested 3.1.0, which requires Java 11. The Javax 2.2 and Jakarta 3.0 APIs use different namespaces. Select the one matching your application's imports and ORM provider.
- Both Persistence API JARs contain APIs, not an ORM implementation. Actual persistence operations require a compatible provider such as Hibernate or EclipseLink, which is not supplied as part of these API profiles.
- GeoHash is the `ch.hsr:geohash` implementation, an alternative to the requested `com.nicta` library. Its package is `ch.hsr.geohash`; it is not a guaranteed drop-in replacement.
- GeoTools `gt-referencing` is used for spatial reference system functionality rather than the originally requested `gt-srs` artifact.
- H3 uses JNI and bundles native code. Native loading was tested on Windows AMD64 only; other platforms or architectures need their own verification.
- Spring Boot 2.7.18 was selected for Java 8. Spring Boot 3.x requires Java 17 or later. The starter JAR is not standalone; its accompanying implementation and dependency JARs must remain available.

## 4. Bundled dependencies

The table below describes the JARs present during the successful test run on November 24, 2026. Counts include the same dependency when it appears in more than one profile; they are not counts of unique artifacts.

| Profile | JARs scanned | Key bundled dependencies |
|---|---:|---|
| `jakarta` | 1 | No external dependency needed for the API smoke test. |
| `javax` | 1 | No external dependency needed for the API smoke test. |
| `postgresql-42.2.18` | 2 | Checker Qual 3.5.0. |
| `postgresql-42.7.1` | 2 | Checker Qual 3.41.0. |
| `geohash` | 1 | Standalone GeoHash JAR. |
| `geotools` | 268 | GeoTools modules, HSQLDB 2.7.2-jdk8, JTS 1.19.0, Guava 32.0.0-jre, units/measurement libraries, imaging and database libraries; PostgreSQL 42.7.1 and Checker Qual 3.41.0 added after the initial test. |
| `guava` | 7 | FailureAccess 1.0.1, ListenableFuture placeholder, JSR305 3.0.2, Checker Qual 3.37.0, Error Prone Annotations 2.21.1, J2ObjC Annotations 2.8. |
| `h3` | 1 | Native components bundled in the H3 JAR. |
| `jedis` | 4 | Commons Pool 2.11.1, SLF4J API 1.7.30 and SLF4J Simple 1.7.30. |
| `spring-redis` | 37 | Spring Boot and Spring Data 2.7.18, Spring Framework 5.3.31, Lettuce 6.1.10.RELEASE, Jedis 3.8.0, Netty 4.1.101.Final, Reactor 3.4.34, Commons Pool 2.11.1 and logging/configuration dependencies. |
| `jts` | 1 | Standalone JTS Core JAR. |
| **Total** | **325** | **JAR scan entries across 11 profiles.** |

Important GeoTools modules include `gt-referencing`, `gt-metadata`, `gt-opengis`, `gt-epsg-hsql` and `gt-main`. The full distribution also contains optional GIS, imaging and database plugins that may not be needed by a particular application. Some optional plugins may require extra software, native libraries or configuration.

## 5. How to use the package

1. Extract `java8-libraries.zip` into a working directory.
2. Select the required library profile(s) for your application.
3. Add the necessary JARs, including dependencies, to the project's compile-time and runtime classpath (or add them as external libraries in your IDE).
4. Run the application using a compatible Java 8 runtime. The package itself is not an executable program and does not install Java.

For example, from the extracted package directory, run an existing compiled class named `MyApp` using Guava on Windows:

```cmd
java -cp ".;lib\guava\*" MyApp
```

If Java is not on PATH, use the full path to an existing or extracted portable runtime instead:

```cmd
"C:\path\to\jre8\bin\java.exe" -cp ".;lib\guava\*" MyApp
```

These examples assume `MyApp.class` is in the current directory and does not require additional profiles. The classpath wildcard includes JARs in one directory, not its subdirectories. For GeoTools, include the directories containing the extracted JARs and the profile-level dependency JARs as needed.

### Common Java imports

| Library | Example import |
|---|---|
| Jakarta Persistence | `jakarta.persistence.Entity` |
| Javax Persistence | `javax.persistence.Entity` |
| PostgreSQL | `java.sql.Connection` (standard JDBC API) |
| GeoHash | `ch.hsr.geohash.GeoHash` |
| GeoTools | `org.geotools.referencing.CRS` |
| Guava | `com.google.common.collect.ImmutableList` |
| H3 | `com.uber.h3core.H3Core` |
| Jedis | `redis.clients.jedis.Jedis` |
| Spring Data Redis | `org.springframework.data.redis.core.RedisTemplate` |
| JTS | `org.locationtech.jts.geom.Geometry` |

A running PostgreSQL or Redis server is necessary for actual remote/database operations, but not for the basic class-loading and initialization tests described below.

## 6. Compatibility test results

All 11 profiles passed individual smoke tests under Java 1.8.0_512 on Windows AMD64. The test program also scanned 63,693 base class entries across 325 JAR scan entries, checking for valid class headers and class-file major versions no greater than 52 (Java 8). Java 9+ multi-release entries and module descriptors were excluded from the bytecode check.

| Profile | Verified operation | Result |
|---|---|---|
| Jakarta Persistence | Load `Entity` annotation. | PASS |
| Javax Persistence | Load `Entity` annotation. | PASS |
| PostgreSQL 42.2.18 | Instantiate driver and validate a JDBC URL. | PASS |
| PostgreSQL 42.7.1 | Instantiate driver and validate a JDBC URL. | PASS |
| GeoHash | Encode coordinates to a nine-character GeoHash. | PASS |
| GeoTools | Decode `EPSG:4326` with longitude-first axis order. | PASS (with warnings) |
| Guava | Create ImmutableList and SettableFuture. | PASS |
| H3 | Load native library and encode coordinates to a cell address. | PASS |
| Jedis | Construct and close a client. | PASS |
| Spring Redis | Load SpringApplication, initialize RedisTemplate, JedisConnectionFactory and Lettuce RedisClient. | PASS |
| JTS | Parse a WKT point and check that it is not empty. | PASS |

Final test output:

```text
All selected smoke tests passed.
```

### Known GeoTools warning

The GeoTools PostgreSQL-backed EPSG plugin attempts to connect to a local database at `localhost:5432`. The test environment had no PostgreSQL server accepting that connection, so the plugin emitted a connection-refused warning. After the missing JDBC driver was added, the GeoTools CRS decoding test completed successfully using an available authority provider.

If the application does not need a PostgreSQL-backed EPSG database, the optional `gt-epsg-postgresql-28.6.1.jar` can be excluded from its classpath. Keep `gt-epsg-hsql-28.6.1.jar` and its HSQLDB dependency for embedded EPSG support. If the PostgreSQL plugin is required, configure its database separately.

### Test limitations

The successful results establish basic compatibility of the tested operations on the stated runtime. They do **not** establish:

- Full application or all-feature compatibility, including all optional GeoTools plugins.
- Compatibility on the exact older Java update `1.8.0_92`.
- Successful PostgreSQL/Redis connections or live read/write operations.
- Compatibility of all profiles combined into one classpath.
- Production security, vulnerability status, or licensing compliance for a specific deployment.

The package is suitable as a starting point for development and integration testing. Run application-level tests on the actual target Java runtime before deployment.

## 7. Dependency conflict precautions

**Do not flatten all profile folders into a single JAR directory or add all profiles to the same classpath indiscriminately.** Some dependencies occur in multiple versions.

| Dependency | Versions in the tested package |
|---|---|
| PostgreSQL JDBC | 42.2.18, 42.7.1 |
| Guava | 32.0.0-jre, 32.1.3-jre |
| SLF4J API | 1.7.30, 1.7.32, 1.7.36 |
| SnakeYAML | 1.30, 2.0 |
| Jakarta Annotation API | 1.3.4, 1.3.5 |
| Checker Qual | 3.5.0, 3.37.0, 3.41.0 |
| JTS Core | 1.19.0 (multiple copies) |
| Jedis | 3.8.0 (multiple copies) |

Choose one compatible version of each shared dependency per application classloader, avoid duplicate JARs and competing logging bindings, and test the resolved classpath. Use only one PostgreSQL JDBC version in a typical application. Follow the application's framework dependency constraints when choosing shared versions; blindly selecting the newest version can introduce incompatibilities.

## 8. Sources, licensing and redistribution

The original downloader obtains the individual artifacts from Maven Central (`https://repo.maven.apache.org/maven2/`) and the GeoTools 28.6.1 binary distribution from the official GeoTools SourceForge project (`https://sourceforge.net/projects/geotools/`). The package contains third-party software; ownership remains with the respective projects and contributors.

Retain applicable license, copyright and notice files, including those shipped with GeoTools and embedded in JARs. Review each component's redistribution terms before distributing the archive. This README is descriptive documentation, not a replacement for third-party license notices or a security assessment. Several versions are older compatibility selections; review security updates before production use.

## 9. Packaging notes

For a library-only delivery, include this README, the complete `lib/` folder and applicable third-party license/notice files. The portable JRE, download scripts, test scripts and original GeoTools ZIP are not required for consuming the extracted JARs. If the recipient needs to reproduce the original smoke tests, provide the original test kit separately (`test.cmd`, `test.ps1`, `Smoke.class`, `downloads.csv` and optionally `Smoke.java`) along with access to a Java 8 runtime. Preserve the profile directory layout.

The inventory and test results in this document reflect the successful test log and original download kit, not a fresh inspection of the final archive. Verify that the files and dependencies are still present when packaging or modifying the libraries.

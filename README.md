# Dutch profile for datasets v2 schema plugin

Dutch profile for datasets v2 (iso19139.nl.geografie.2.0.0).

## Installing the plugin

### GeoNetwork version to use with this plugin

Use GeoNetwork 3.10.

### Adding the plugin to the source code

The best approach is to add the plugin as a submodule into GeoNetwork schema module.

```
cd schemas
git submodule add -b 3.10.x https://github.com/metadata101/iso19139.nl.geografie.2.0.0 iso19139.nl.geografie.2.0.0
```

Add the new module to the schema/pom.xml:

```
  <module>iso19139</module>
  <module>iso19139.nl.geografie.2.0.0</module>
</modules>
```

Add the dependency in the web module in web/pom.xml:

```
<dependency>
  <groupId>${project.groupId}</groupId>
  <artifactId>schema-iso19139.nl.geografie.2.0.0</artifactId>
  <version>${project.version}</version>
</dependency>
```

Add the module to the webapp in web/pom.xml:

```
<execution>
  <id>copy-schemas</id>
  <phase>process-resources</phase>
  ...
  <resource>
    <directory>${project.basedir}/../schemas/iso19139.nl.geografie.2.0.0/src/main/plugin</directory>
    <targetPath>${basedir}/src/main/webapp/WEB-INF/data/config/schema_plugins</targetPath>
  </resource>
```

### Adding editor configuration

Editor configuration in GeoNetwork 3.4.x is done in `schemas/iso19139.nl.geografie.2.0.0/src/main/plugin/iso19139.nl.geografie.2.0.0/layout/config-editor.xml` inside each view. Default values are the following:

      <sidePanel>
        <directive data-gn-onlinesrc-list=""/>
        <directive gn-geo-publisher=""
                   data-ng-if="gnCurrentEdit.geoPublisherConfig"
                   data-config="{{gnCurrentEdit.geoPublisherConfig}}"
                   data-lang="lang"/>
        <directive data-gn-validation-report=""/>
        <directive data-gn-suggestion-list=""/>
        <directive data-gn-need-help="user-guide/describing-information/creating-metadata.html"/>
      </sidePanel>

### Build the application 

Once the application is build, the war file contains the schema plugin:

```
$ mvn clean install -Penv-prod
```

### Deploy the profile in an existing installation

After building the application, it's possible to deploy the schema plugin manually in an existing GeoNetwork installation:

- Copy the content of the folder schemas/iso19139.nl.geografie.2.0.0/src/main/plugin to INSTALL_DIR/geonetwork/WEB-INF/data/config/schema_plugins/iso19139.nl.geografie.2.0.0

- Copy the jar file schemas/iso19139.nl.geografie.2.0.0/target/schema-iso19139.nl.geografie.2.0.0-3.10.jar to INSTALL_DIR/geonetwork/WEB-INF/lib.

If there's no changes to the profile Java code or the configuration (config-spring-geonetwork.xml), the jar file is not required to be deployed each time.

### Setup xml snippets

The editor uses a snippet-directive for the conformance field which allows to select a snippet from the catalogue, to be introduced at that location.
Unzip [snippets.zip](https://github.com/metadata101/iso19139.nl.geografie.2.0.0/raw/3.10.x/snippets.zip) to a folder, use import (select type 'subtemplate') to import all the files into the catalogue.

### Setup thesauri

The editor uses a number of thesauri as pick lists for various fields. These thesauri can be downloaded from the INSPIRE registry and Nationaalgeoregister.nl.

  - [INSPIRE themes](http://inspire.ec.europa.eu/theme),

  - [Priority Dataset](http://inspire.ec.europa.eu/metadata-codelist/PriorityDataset),

  - [Protocol Value](http://inspire.ec.europa.eu/metadata-codelist/ProtocolValue),

  - [Spatial Data Service Category](https://inspire.ec.europa.eu/metadata-codelist/SpatialDataServiceCategory),

  - [Quality Of Service Criteria](http://inspire.ec.europa.eu/metadata-codelist/QualityOfServiceCriteria),

  - [Spatial Scope](http://inspire.ec.europa.eu/metadata-codelist/SpatialScope),

  - [Spatial DataService Type](https://inspire.ec.europa.eu/metadata-codelist/SpatialDataServiceType).

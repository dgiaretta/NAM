# NAM interactions
## Normal ingest of digital objects
Much of this, apart from the Catalogue, is documented in the Records Management  document.

The Access Register could be part of the CatalogueDB. Digital Objects which have been uploaded to Eternal would have this flagged in the appropriate field, to distinguish objects in Eternal from other objects e.g.
1. objects which are waiting for additional information to be aquired before they can be uploaded to Eternal
2. Objects which are not suitable for preservation, and so will not be uploaded to Eternal.

```mermaid
sequenceDiagram
participant PhysicalSource
participant DigitalSource
actor NAM
participant IngestSpreadsheet
participant AccessionRegister
participant CatalogueDB@{ "type": "database" }
participant EternalSystem
participant GeneralFrontEnd
participant SpecialisedFrontEnd
DigitalSource->>NAM: Data plus Hashes
NAM->>IngestSpreadsheet: Create entries
IngestSpreadsheet->>AccessionRegister: Add entries 
IngestSpreadsheet->>EternalSystem: Ingest for Preservation
IngestSpreadsheet->>CatalogueDB: Add entries to catalogue and indicate that entries are in Eternal
EternalSystem->>CatalogueDB: Add UUIDs for entries
NAM->>CatalogueDB: Add catalogue information for searching
GeneralFrontEnd->>CatalogueDB: Query holdings
CatalogueDB->>GeneralFrontEnd: Display response
```

## Digitisation of physical objects
Papers can be scanned as PDFs and 3-D objects stored in a more complex file.

Papers may be kept after digitisation e.g. old manuscripts. In this case that  physical paper will be treated as described below, and linked to the digital scan.

```mermaid
sequenceDiagram
participant PhysicalSource
participant DigitalSource
actor NAM
participant IngestSpreadsheet
participant AccessionRegister
participant CatalogueDB@{ "type": "database" }
participant EternalSystem
participant GeneralFrontEnd
participant SpecialisedFrontEnd
Note left of PhysicalSource: Digitisation of physical object
PhysicalSource->>DigitalSource: Scan object
Note right of DigitalSource: continue as before
```
## Dealing with physical objects
Physical objects which have not yet been, or which may never be, scanned,  are recorded in the Accession Register and the CatalogueDB.

Because these are physical objects they have a physical location e.g. the records room at NAM, with rack and shelf location or even a geographical location such as an address or GPS location.

```mermaid
sequenceDiagram
participant PhysicalSource
participant DigitalSource
actor NAM
participant IngestSpreadsheet
participant AccessionRegister
participant CatalogueDB@{ "type": "database" }
participant EternalSystem
participant GeneralFrontEnd
participant SpecialisedFrontEnd
par 
Note left of PhysicalSource: Record that NAM has physical objects
PhysicalSource->>NAM: Data plus Locations
NAM->>IngestSpreadsheet: Create entries with description of objects and location
IngestSpreadsheet->>AccessionRegister: Add entries 
IngestSpreadsheet->>CatalogueDB: Add entries to catalogue and indicate location of entries
NAM->>CatalogueDB: Add catalogue information for searching if needed
end
```

## Users querying the catalogue
There may be fields and objects which general users are not allowed to see or download.

Paying via a special payment system may allow such users additional access.

```mermaid
sequenceDiagram
participant PhysicalSource
participant DigitalSource
actor NAM
participant IngestSpreadsheet
participant AccessionRegister
participant CatalogueDB@{ "type": "database" }
participant EternalSystem
participant GeneralFrontEnd
participant SpecialisedFrontEnd 
Note right of CatalogueDB: General users querying catalogue
loop
  GeneralFrontEnd->>CatalogueDB: Query holdings
  CatalogueDB->>GeneralFrontEnd: Display response
end
```



## Archivists querying the catalogue
Archivists will be able to see additional details
```mermaid
sequenceDiagram
participant PhysicalSource
participant DigitalSource
actor NAM
participant IngestSpreadsheet
participant AccessionRegister
participant CatalogueDB@{ "type": "database" }
participant EternalSystem
participant GeneralFrontEnd
participant SpecialisedFrontEnd
Note right of CatalogueDB: Archivist view of the archive holdings
loop
  SpecialisedFrontEnd->>CatalogueDB: Query holdings to see archival details
  CatalogueDB->>SpecialisedFrontEnd: Display response
end
```

## Synchronising the catalogue with Eternal
Besides normal backups it would be sensible to synchronise the CatalogueDB with Eternal
```mermaid
sequenceDiagram
participant PhysicalSource
participant DigitalSource
actor NAM
participant IngestSpreadsheet
participant AccessionRegister
participant CatalogueDB@{ "type": "database" }
participant EternalSystem
participant GeneralFrontEnd
participant SpecialisedFrontEnd
Note left of PhysicalSource: Backup catalogue to Eternal
CatalogueDB-->>EternalSystem: Backup the catalogue entries
EternalSystem-->>CatalogueDB: Update catalogue wrt preservation activities
```


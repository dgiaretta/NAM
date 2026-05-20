# NAM
```mermaid
sequenceDiagram
participant PhysicalSource
participant DigitalSource
participant NAM
participant IngestSpreadsheet
participant AccessionRegister
participant CatalogueDB
participant EternalSystem
participant GeneralFrontEnd
participant SpecialisedFrontEnd
Note left of PhysicalSource: Normal ingest of digital holdings
DigitalSource->>NAM: Data plus Hashes
NAM->>IngestSpreadsheet: Create entries
IngestSpreadsheet->>AccessionRegister: Add entries 
IngestSpreadsheet->>EternalSystem: Ingest for Preservation
IngestSpreadsheet->>CatalogueDB: Add entries to catalogue and indicate that entries are in Eternal
EternalSystem->>CatalogueDB: Add UUIDs for entries
NAM->>CatalogueDB: Add catalogue information for searching
GeneralFrontEnd->>CatalogueDB: Query holdings
CatalogueDB->>GeneralFrontEnd: Display response
Note left of PhysicalSource: Recording that NAM has physical objects
PhysicalSource->>NAM: Data plus Hashes
NAM->>IngestSpreadsheet: Create entries with description of objects and location
IngestSpreadsheet->>AccessionRegister: Add entries 
IngestSpreadsheet->>CatalogueDB: Add entries to catalogue and indicate location of entries
NAM->>CatalogueDB: Add catalogue information for searching if needed
GeneralFrontEnd->>CatalogueDB: Query holdings
CatalogueDB->>GeneralFrontEnd: Display response
Note left of PhysicalSource: Digitisation of physical object
PhysicalSource->>DigitalSource: Scan object
Note right of DigitalSource: continue as before
Note left of PhysicalSource: Archivist view of the archive holdings
SpecialisedFrontEnd->>CatalogueDB: Query holdings to see archival details
CatalogueDB->>SpecialisedFrontEnd: Display response



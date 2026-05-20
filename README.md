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
Note left of DigitalSource: Normal ingest of digital holdings
DigitalSource->>NAM: Data plus Hashes
NAM->>IngestSpreadsheet: Create entries
IngestSpreadsheet->>AccessionRegister: Add entries 
IngestSpreadsheet->>EternalSystem: Ingest for Preservation
IngestSpreadsheet->>CatalogueDB: Add entries to catalogue and indicate that entries are in Eternal
EternalSystem->>CatalogueDB: Add UUIDs for entries
NAM->>CatalogueDB: Add catalogue information for searching
GeneralFrontEnd->>CatalogueDB: Query holdings
CatalogueDB->>GeneralFrontEnd: Display response


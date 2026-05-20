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
DigitalSource-->>NAM: Data plus Hashes
NAM-->>IngestSpreadsheet: Create entries
IngestSpreadsheet-->>EternalSystem: Ingest for Preservation

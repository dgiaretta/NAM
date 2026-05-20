# NAM interactions
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
end
par 
Note left of PhysicalSource: Record that NAM has physical objects
PhysicalSource->>NAM: Data plus Hashes
NAM->>IngestSpreadsheet: Create entries with description of objects and location
IngestSpreadsheet->>AccessionRegister: Add entries 
IngestSpreadsheet->>CatalogueDB: Add entries to catalogue and indicate location of entries
NAM->>CatalogueDB: Add catalogue information for searching if needed
end
par  
Note right of CatalogueDB: General users querying catalogue
loop
  GeneralFrontEnd->>CatalogueDB: Query holdings
  CatalogueDB->>GeneralFrontEnd: Display response
end
end
par
Note left of PhysicalSource: Digitisation of physical object
PhysicalSource->>DigitalSource: Scan object
Note right of DigitalSource: continue as before
end
par
Note right of CatalogueDB: Archivist view of the archive holdings
loop
  SpecialisedFrontEnd->>CatalogueDB: Query holdings to see archival details
  CatalogueDB->>SpecialisedFrontEnd: Display response
end
end
par
Note left of PhysicalSource: Backup catalogue to Eternal
CatalogueDB-->>EternalSystem: Backup the catalogue entries
EternalSystem-->>CatalogueDB: Update catalogue wrt preservation activities
end


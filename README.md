# NAM interactions
## Normal ingest of digital objects
Much of this, apart from the Catalogue, is documented in the Records Management  document.


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
DigitalSource->>NAM: 1. Data plus Hashes
NAM->>IngestSpreadsheet: 2. Create entries
IngestSpreadsheet->>AccessionRegister: 3. Add entries 
IngestSpreadsheet->>EternalSystem: 4. Ingest for Preservation
IngestSpreadsheet->>CatalogueDB: 5. Add entries to catalogue and indicate that entries are in Eternal
EternalSystem->>CatalogueDB: 6. Add UUIDs for entries
NAM->>CatalogueDB: 7. Add catalogue information for searching
GeneralFrontEnd->>CatalogueDB: 8. Query holdings
CatalogueDB->>GeneralFrontEnd: 9. Display response
```

The steps can be described as follows:
1. The source of the digital objects (e.g. a Ministry) sends to NAM the digital objects plus the spreadsheet of hashes created using the small command line provided (assuming a Windows PC) either by sending by email or using OwnCloud
2.  NAM then creates the ingest spreadsheet which includes all the information needed for preservation. The spreadsheet could also include the catalogue information which Eternal may or may not import
3. The spreadsheet entries are appended to the Accession Register - this ensure that no matter what else happens, there is a record that these digital objects have been accepted by NAM. The Access Register could be part of the CatalogueDB. Digital Objects which have been uploaded to Eternal would have this flagged in the appropriate field, to distinguish objects in Eternal from other objects e.g. <br/> 1. objects which are waiting for additional information to be aquired before they can be uploaded to Eternal or <br/> 2. Objects which are not suitable for preservation, and so will not be uploaded to Eternal.
4. The digital objects are ingested into Eternal for preservation, accompanied by the spreadsheet which adds information needed to create the AIP.
5. The spreadsheet entries are appended to the CatalogueDB database. Note that the Accession Register may also be part of this database. A flag is set to indicate that the digital objects have been ingested. The database should be accessible via one or more web interfaces. NAM prefers that the CatalogueDB is held ona NAM controlled machine.
6. When Eternal has created the AIPs, the UUID, which identifies each AIP, is added to the CatalogueDB so that there is a link between the catalogue and the AIPs.
7. Additional information may be added to the CatalogueDB by NAM in order to make the information more easily searchable. TNA uses a 7 level set of entries, which NAM could use.<br/>Note that a full text search is possible locally but this will require that SOLR, for example, is run locally to index all the digital objects as well as the "metadata".
8. The CatalogueDB can be used to query the contents of NAM
9. The CatalogueDB returns results
  

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
Note left of PhysicalSource: Digitisation of<br/>physical object
PhysicalSource->>DigitalSource: 1. Scan object
Note right of DigitalSource: 2. continue<br/>as before
```

The sequence is
1. a physical object is scanned to create a digital object.
2. the digital object can be ingested as described above

If the physical object is not discarded after scanning e.g. when the paper manuscript has specific value as a phyical object, then it is dealt with in the way described below, but with a link to the digital scan.

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
 
Note left of PhysicalSource: Record that<br/>NAM has<br/>physical objects
PhysicalSource->>NAM: 1. Data about the physical objects plus Locations
NAM->>IngestSpreadsheet: 2. Create entries with description of objects and location
IngestSpreadsheet->>AccessionRegister: 3. Add entries 
IngestSpreadsheet->>CatalogueDB: 4. Add entries to catalogue and indicate location of entries
NAM->>CatalogueDB: 5. Add catalogue information for searching if needed

```

The sequence is as follows:
1. The source of (or holder) of the physical objects send information about the objects plus their locations to NAM
2. NAM creates an ingest spreadsheet for these objects
3. The entries are added to the Accession Database.
4. The entries are also added to the CatalogueDB - this must include the fact that these are physical objects and their locations.
5. NAM can add additional information to the catalogue to facilitate searches.

Note that querying the catalogue will show the information about the physical objects and may indicate whether/how the user may view the physical object.

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
Note right of CatalogueDB: General users<br/>querying catalogue
loop
  GeneralFrontEnd->>1. CatalogueDB: Query holdings
  CatalogueDB->>GeneralFrontEnd: 2. Display response
end
```

The sequence is:
1. A user can use a Web interface (or use a REST API) to query the catalogue. There may be a payment required, depending on the request
2. The CatalogueDB front end responds to the user.



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
Note right of CatalogueDB: Archivist view<br/>of the archive holdings
loop
  SpecialisedFrontEnd->>CatalogueDB: Query holdings to see archival details
  CatalogueDB->>SpecialisedFrontEnd: Display response
end
```

The sequence is:
1. An archivist can use a specialised Web interface (or use a REST API) to query the catalogue. 
2. The CatalogueDB front end responds to the archivist.

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
Note left of CatalogueDB: Backup<br/>catalogue to<br/>Eternal
CatalogueDB-->>EternalSystem: Backup the catalogue entries
EternalSystem-->>CatalogueDB: Update catalogue wrt preservation activities
```

The sequence is:
1. A regular synchronisation task updates the copy of the CatalogueDB on Eternal
2. If additional information has been created by Eternal during its preservation activities, then this can be inserted into the CatalogueDB for conveience.

Use https://www.mermditor.dev/editor for PDF


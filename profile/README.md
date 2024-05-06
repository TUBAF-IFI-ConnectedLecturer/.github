# Connected Lectures

Aggregation von OER Meta-Daten aus unterschiedlichen Lehr-Lern-Materialien sächsischer Hochschulen

```mermaid
flowchart TD

    classDef green fill:#5bd21c
    classDef yellow fill:#ffd966
    classDef gray fill:#bcbcbc

    subgraph Materialidentifikation
    OPAL[(OPAL)] --> OPAL_QUERY(OPAL Query)
    OPAL_QUERY:::green
    OPAL_QUERY --> OPAL_REPOS[(OPAL OER\nKurs\nVerzeichnis)]
    OPAL_QUERY --> OPAL_FILES[(Einzel\nDateien\nVerzeichnis)]
    LIA_IDENT(Liascript Identifikation)
    GITHUB[(Github)] --> |Github API| LIA_IDENT:::green
    LIA_IDENT --> |Dateisuche| LIA_FILES[(Lia\nDateien)]
    LIA_REPOS --> LIA_FILES
    LIA_IDENT --> |Reposuche| LIA_REPOS[(Lia\nRepos)]
    LIA_FILES --> FEATURE_EXTRACTION_LIA(Metadatenextraktion Dokument)
    FEATURE_EXTRACTION_LIA:::green
    end

    subgraph Materialbereitstellung
    FILE_DOWNLOAD(Datei Download)
    FILE_DOWNLOAD:::green --> PDFS[(pdf\n Dateien)]
    FILE_DOWNLOAD --> OFFICE[(.docx,\n.pptx, ...)]
    PDFS --> TEXTEXTRAKTION_OPAL(Textextraktion)
    OFFICE --> TEXTEXTRAKTION_OPAL(Textextraktion)
    TEXTEXTRAKTION_OPAL:::yellow
    TEXTEXTRAKTION_OPAL --> TEXTPREPROCESSING_OPAL(Textbereinigung)
    GITHUB_DOWNLOAD(Github Download)
    GITHUB_DOWNLOAD:::green --> TEXTEXTRAKTION_LIA(Textextraktion)
    TEXTEXTRAKTION_LIA:::yellow
    TEXTEXTRAKTION_LIA --> TEXTPREPROCESSING_LIA(Textbereinigung)
    TEXTPREPROCESSING_LIA:::yellow
    METADATENEXTRAKTION(Metadatenextraktion \n Inhalt)
    METADATENEXTRAKTION:::yellow
    end

    subgraph Textanalyse
    KEYWORDEXTRAKTION(Keywordextraktion) --> KLASSIFIKATION(Klassifikation)
    KLASSIFIKATION --> LLM(LLM)
    LLM:::yellow
    KLASSIFIKATION --> ?
     end

    OPAL_FILES -->  FILE_DOWNLOAD
    FEATURE_EXTRACTION_LIA -->  GITHUB_DOWNLOAD
    TEXTPREPROCESSING_OPAL --> KEYWORDEXTRAKTION
    TEXTPREPROCESSING_LIA --> KEYWORDEXTRAKTION

    class Materialidentifikation,Materialbereitstellung,Textanalyse gray
```

# python_kladd

Denne repoen inneholder Python-moduler i mappen `modules`.

Modulene i `modules` er laget for å kunne gjenbrukes fra ArcGIS Online, for eksempel ved å laste ned en `.py`-fil og importere funksjoner i en notebook.

Eksempel:

```python
from modules.test import hello_world
```

Retningslinjer:

- Mappen `modules` skal inneholde moduler med funksjoner som kan brukes fra ArcGIS Online.
- Hold modulene enkle å importere og gjenbruke.
- Ikke legg inn sensitiv informasjon i denne repoen eller i `modules`.
- Ikke hardkod nøkler, passord, tokens, klienthemmeligheter eller andre hemmelige verdier i kodefilene.

Hvis kode fra repoen skal brukes i ArcGIS Online, bør modulene være selvstendige og ikke være avhengige av lokale filer med sensitivt innhold.
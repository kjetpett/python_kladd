# python_kladd

Denne repoen inneholder enkle Python-moduler i mappen `modules` som er ment brukt fra notebooker i ArcGIS Online.

Bruken er basert på at notebooken laster ned en bestemt `.py`-fil direkte fra GitHub og importerer den lokalt i notebook-miljøet. Dette er bevisst holdt enkelt, og repoen er ikke satt opp som en installbar Python-pakke for `pip`.

## Hvordan modulene brukes i ArcGIS Online

Typisk bruk er:

1. En notebook bygger en GitHub-URL mot `modules/<modulnavn>.py`.
2. Filen lastes ned til et lokalt cache-område i notebooken, for eksempel under `/arcgis/home/modules_cache/<commit>/`.
3. Notebooken importerer modulen derfra.
4. Ved behov kan notebooken peke på en bestemt commit for å låse en konkret versjon.

Eksempel med `main` som standard og valgfri commit:

```python
import os
import urllib.request
import importlib.util

def load_github_module(repo_path, owner="kjetpett", repo="python_kladd", commit=None):
	commit = commit or "main"
	local_file = f"/arcgis/home/modules_cache/{commit}/{repo_path}"
	os.makedirs(os.path.dirname(local_file), exist_ok=True)

	url = f"https://raw.githubusercontent.com/{owner}/{repo}/{commit}/{repo_path}"
	urllib.request.urlretrieve(url, local_file)

	module_stub = repo_path.replace("/", "_").removesuffix(".py")
	module_name = f"{module_stub}_{commit[:8]}"

	spec = importlib.util.spec_from_file_location(module_name, local_file)
	module = importlib.util.module_from_spec(spec)
	spec.loader.exec_module(module)
	return module

mod = load_github_module("modules/hello_world.py")
print(mod.hello_world("ArcGIS"))
```

Eksempel med en bestemt commit:

```python
mod = load_github_module(
	"modules/hello_world.py",
	commit="777cb06ed6db370aa1cb7bfb55e74bfbb4df7df2",
)

print(mod.hello_world("ArcGIS"))
```

## Retningslinjer for `modules`

- `modules` skal inneholde små, selvstendige Python-moduler som kan brukes fra ArcGIS Online-notebooker.
- Modulene bør ha få og tydelige avhengigheter.
- Modulene bør ikke være avhengige av lokale filer som bare finnes på en bestemt maskin.
- Endringer i modulene bør være bakoverkompatible når det er praktisk mulig, siden notebooker kan peke på ulike commits.

## Sensitiv informasjon

Det skal ikke ligge sensitiv informasjon i denne repoen eller i `modules`.

Ikke legg inn:

- passord
- API-nøkler
- tokens
- klienthemmeligheter
- connection strings
- brukerspesifikke hemmelige verdier

Hvis en notebook trenger hemmeligheter, skal de hentes fra en trygg kilde utenfor denne repoen.
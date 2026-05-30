# SFF Build Checker Agent 

Egy intelligens, LangGraph-alapú adatközpontú (Data-driven) ágens, amely Small Form Factor (SFF) számítógépek tervezésében és validálásában nyújt segítséget. 

A projekt legfőbb architekturális sajátossága a **Szigorú Zéró-Tudás (Zero-Knowledge) megközelítés**: az ágens nem használhatja a saját nyelvi modelljébe kódolt (hallucinációra hajlamos) hardveres ismereteit. Minden adatot valós időben, külső API-kon keresztül szerez be, validál, és a lokális memóriájában (State) tárol.

## Főbb Funkciók

* **Zéró-Tudás Validáció:** Nincs LLM hallucináció. A méreteket és a TDP értékeket valós időben kaparja le a hivatalos specifikációs oldalakról (TechPowerUp, PCPartPicker).
* **Web Scraper (V2):** Tavily alapú keresés és ScraperAPI prémium proxy hálózat a Cloudflare bot-védelmek megkerülésére, kiegészítve robusztus Regex adatkinyeréssel.
* **Dinamikus PSU Kalkulátor:** Mérnöki számítás alapján összegzi a build TDP-jét, hozzáadja az alaprendszer fogyasztását, és 20% biztonsági tartalékkal kalkulál ajánlott tápegységet.
* **Fórum Hangulatelemzés (Sentiment Analysis):** Valós idejű Reddit véleménykutatás a `Hugging Face Inference API` és a DistilBERT modell segítségével a közösségi megbízhatóság mérésére.
* **State Management:** Helyi adatbázis menedzselése a build állapotának követésére (alkatrészek hozzáadása, cseréje, törlése memórián belül).
* **Valós idejű Valutaváltó:** Hardverárak konverziója USD, EUR, HUF és RSD között az ExchangeRate-API segítségével.

## Technológiai Stack

* **Core Logic:** LangGraph, LangChain
* **LLM:** Google Gemini (Gemini 3.1 Pro / Flash Lite)
* **NLP / Sentiment:** Hugging Face (`lxyuan/distilbert-base-multilingual-cased-sentiments-student`)
* **Adatgyűjtés:** BeautifulSoup4, Requests, Tavily Search API, ScraperAPI
* **Nyelv:** Python 3.12

## Használat

A projekt teljesen "önellátó" (self-contained) egyetlen Jupyter Notebook fájlban, dedikált lokális telepítés nem szükséges.

1. Nyisd meg az `NLP_Beadando_Agent.ipynb` fájlt (Google Colab-ban vagy lokális Jupyter környezetben).
2. A futtatáshoz szükséges könyvtárakat (`langchain`, `tavily`, stb.) a legelső kódcella automatikusan telepíti.
3. A környezeti változókat (API kulcsok) a második kódcellában találod, ezeket szükség esetén itt tudod módosítani.
4. Futtasd le a cellákat sorban a rendszer inicializálásához és a tesztek indításához.

## Tesztelés

A notebook tartalmaz egy robusztus, 23 esetből álló tesztbankot (Unit tesztek, Edge Case-ek, Adversarial Promptek), amely automatikusan lefut, és validálja a következőket:
* Single és Multi-Tool adatgyűjtés pontossága.
* State Management integritása (State Bleed elkerülése).
* Zéró-Tudás szabály betartása (fiktív kártyák elutasítása).
* Fizikai peremfeltételek felismerése (pl. szűk, kétkamrás "sandwich" házak hűtési kihívásai).

## Készítette

**Raffai Sajti Dávid** Szegedi Tudományegyetem (SZTE)  
*NLP (Természetes Nyelvfeldolgozás) beadandó projekt*


# **Detekce karet při turnajích v pokru**

Aplikace pro detekci hracích karet v rukách hráčů a na stole během pokerových turnajů.

**Autor:** Vladyslav Kovalets

Aplikace generuje textový soubor se seznamem detekovaných karet a vytváří upravené video s vloženými ikonami těchto karet. Podrobnější výsledky detekce, včetně typu karty, souřadnic a jistoty rozpoznání, jsou uloženy do JSON souboru.

#### Obsah
1. [Adresářová struktura](#adresářová-struktura)
2. [Instalace závislostí](#instalace-závislostí)
3. [Použití](#použití)
    - [Detekce karet v rukách hráče](#1-detekce-karet-v-rukách-hráče)
    - [Detekce karet na stole](#2-detekce-karet-na-stole)
4. [Poznámky](#poznámky)
    - [JSON soubor](#json-soubor)
    - [Výstupní video](#výstupní-video)

## Adresářová struktura
```
├── card_icons
├── input
│   ├── players
│   │   ├── player_1.mp4
│   │   ├── player_2.mp4
│   │   ├── player_3.mp4
│   │   ├── player_4.mp4
│   │   └── player_5.mp4  
│   └── tables
│       ├── table_1.mp4
│       ├── table_2.mp4
│       ├── table_3.mp4
│       └── table_4.mp4
├── model
│   └── poker_card_detector.pt
├── output
│   ├── players
│   │   ├── framewise_detected_players_cards.json
│   │   ├── player_1.mp4
│   │   ├── player_2.mp4
│   │   ├── player_3.mp4
│   │   ├── player_4.mp4
│   │   ├── player_5.mp4
│   │   └── players_cards_list.txt
│   └── table
│       ├── framewise_detected_table_cards.json
│       ├── table_1.mp4
│       └── table_cards_list.txt
├── player_cards_detector.py
├── README.md
├── requirements.txt
└── table_cards_detector.py
```

- `card_icons` - ikony karet pro vložení do výstupního videa.
- `input` - adresář obsahující vstupní videa hráčů a stolů.
- `model` - složka obsahující předtrénovaný model.
- `output` - adresář s výstupními daty a videi.
- `player_cards_detector.py` - skript pro detekci karet v rukách hráče.
- `table_cards_detector.py` - skript pro detekci karet na stole.

## Instalace závislostí

Před spuštěním aplikace je nutné nainstalovat požadované knihovny. Můžete je nainstalovat pomocí souboru `requirements.txt`:

```bash
pip install -r requirements.txt
```

## Použití

### 1. Detekce karet v rukách hráče

Skript `player_cards_detector.py` slouží k detekci pokerových karet v rukách hráče. Zpracovává video z kamery zabudované do hrany stolu, která zachycuje moment, kdy se hráč dívá na své karty. Skript akceptuje jako vstup buďto jedno video, nebo více videí z jedné hry. V případě výběru více videí z jedné hry se předpokládá, že každé video zachycuje akce jiného hráče u stolu.

#### Parametry:
- `--source`: cesta k videu ve formátu .mp4

#### Příklad spuštění:

```bash
python player_cards_detector.py --source input/players/*
```

#### Příklad výstupu:
```
Using cache found in /home/xkoval21/.cache/torch/hub/ultralytics_yolov5_master
YOLOv5 🚀 2024-2-26 Python-3.8.10 torch-2.1.0+cu121 CUDA:0 (NVIDIA GeForce RTX 2060, 6144MiB)

Fusing layers...
Model summary: 212 layers, 21059025 parameters, 0 gradients, 48.5 GFLOPs
Adding AutoShape...
Processing video: input/players/player_1.mp4
Processed video saved to output/players/player_1.mp4
Processing video: input/players/player_2.mp4
Processed video saved to output/players/player_2.mp4
Processing video: input/players/player_3.mp4
Processed video saved to output/players/player_3.mp4
Processing video: input/players/player_4.mp4
Processed video saved to output/players/player_4.mp4
Processing video: input/players/player_5.mp4
Processed video saved to output/players/player_5.mp4
Detected cards information saved to output/players/framewise_detected_players_cards.json
List of cards saved to output/players/players_cards_list.txt
```


**Obsah souboru `players_cards_list.txt`**

Seznam karet v rukách hráčů. Každý řádek obsahuje informace o hráči a jeho kartách.

```
Hráč 1: 8C, QS.
Hráč 2: 9H, JC.
Hráč 3: 7S, AS.
Hráč 4: 6S, 9S.
Hráč 5: JH, QC.
```

## 2. Detekce karet na stole

Skript `table_cards_detector.py` slouží k detekci pokerových karet na herním stole. Zpracovává video z kamery, která je umístěná nad stolem a zachycuje činnosti krupiéra při manipulaci s kartami. Skript akceptuje jako vstup pouze jedno video.

#### Parametry:
- `--source` - cesta k videu ve formátu .mp4

#### Příklad spuštění:

```bash
python table_cards_detector.py --source input/tables/table_1.mp4
```

#### Příklad výstupu:
```
Using cache found in /home/xkoval21/.cache/torch/hub/ultralytics_yolov5_master
YOLOv5 🚀 2024-2-26 Python-3.8.10 torch-2.1.0+cu121 CUDA:0 (NVIDIA GeForce RTX 2060, 6144MiB)

Fusing layers...
Model summary: 212 layers, 21059025 parameters, 0 gradients, 48.5 GFLOPs
Adding AutoShape...
Processing video: input/tables/table_1.mp4
Processed video saved to output/table/table_1.mp4
Detected cards information saved to output/table/framewise_detected_table_cards.json
List of cards saved to output/table/table_cards_list.txt
```

**Obsah souboru `table_cards_list.txt`**

Seznam karet na stole.

``` Karty na stole: AC, KC, 5H, 2C, 4D. ```


## Poznámky

Každý ze skriptů `player_cards_detector.py` a `table_cards_detector.py` generuje výstupní JSON soubor a upravené video. 



#### JSON soubor
Výstupní JSON soubor obsahuje podrobné informace o detekovaných kartách. Formát souboru je následující:

```json
{
    "frame_id": 42,
    "timestamp": 1.37,
    "cards_detected_one_corner": [
        "6S",
        "9S"
    ],
    "cards_detected_two_corners": [],
    "cards_info": [
        {
            "card_id": 24,
            "name": "6S",
            "x_coord": 654.62,
            "y_coord": 318.49,
            "confidence": 0.9
        },
        {
            "card_id": 36,
            "name": "9S",
            "x_coord": 648.39,
            "y_coord": 284.21,
            "confidence": 0.82
        }
    ]
},
```

- `frame_id` - číslo snímku.
- `timestamp` - časový údaj.
- `cards_detected_one_corner` - seznam karet detekovaných pouze jedním rohem.
- `cards_detected_two_corners` - seznam karet detekovaných dvěma rohy.
- `cards_info` - informace o detekovaných kartách.
  - `card_id` - identifikátor karty.
  - `name` - název karty.
  - `x_coord` - x-ová souřadnice detekovaného rohu karty.
  - `y_coord` - y-ová souřadnice detekovaného rohu karty.
  - `confidence` - jistota detekce.

#### Výstupní video

Upravené video obsahuje vložené ikony detekovaných karet. Ikony jsou umístěny v levém horním rohu obrazovky.

#### Zkratky karet a jejich význam


| Zkratka |     Význam     |
|---------|----------------|
| 2C      | Dvojka kříže   |
| 2D      | Dvojka káry    |
| 2H      | Dvojka srdce   |
| 2S      | Dvojka piky    |
| 3C      | Trojka kříže   |
| 3D      | Trojka káry    |
| 3H      | Trojka srdce   |
| 3S      | Trojka piky    |
| 4C      | Čtyřka kříže   |
| 4D      | Čtyřka káry    |
| 4H      | Čtyřka srdce   |
| 4S      | Čtyřka piky    |
| 5C      | Pětka kříže    |
| 5D      | Pětka káry     |
| 5H      | Pětka srdce    |
| 5S      | Pětka piky     |
| 6C      | Šestka kříže   |
| 6D      | Šestka káry    |
| 6H      | Šestka srdce   |
| 6S      | Šestka piky    |
| 7C      | Sedmička kříže |
| 7D      | Sedmička káry  |
| 7H      | Sedmička srdce |
| 7S      | Sedmička piky  |
| 8C      | Osmička kříže  |
| 8D      | Osmička káry   |
| 8H      | Osmička srdce  |
| 8S      | Osmička piky   |
| 9C      | Devítka kříže  |
| 9D      | Devítka káry   |
| 9H      | Devítka srdce  |
| 9S      | Devítka piky   |
| 10C     | Desítka kříže  |
| 10D     | Desítka káry   |
| 10H     | Desítka srdce  |
| 10S     | Desítka piky   |
| JC      | Kluk kříže     |
| JD      | Kluk káry      |
| JH      | Kluk srdce     |
| JS      | Kluk piky      |
| QC      | Dáma kříže     |
| QD      | Dáma káry      |
| QH      | Dáma srdce     |
| QS      | Dáma piky      |
| KC      | Král kříže     |
| KD      | Král káry      |
| KH      | Král srdce     |
| KS      | Král piky      |
| AC      | Eso kříže      |
| AD      | Eso káry       |
| AH      | Eso srdce      |
| AS      | Eso piky       |
# Automatische Zählung von Fahrrädern in Vorbeifahr-Videos

Dieses Repository enthält die Implementierung einer Einzelarbeit im Modul *Artificial Intelligence*.  
Ziel der Arbeit ist die automatische Schätzung der Anzahl von Fahrrädern in einer Veloparkierungsanlage auf Basis kurzer Videoaufnahmen, bei denen die Kamera seitlich an der Anlage vorbeifährt.

Da nie alle Fahrräder gleichzeitig sichtbar sind, wird eine Kombination aus Objekterkennung und Multi-Object-Tracking eingesetzt.

---

## Projektübersicht

**Problemstellung**  
Wie viele Fahrräder befinden sich insgesamt in einer Veloparkierungsanlage, wenn diese nur über eine kurze Vorbeifahrt gefilmt wird?

**Herausforderung**
- Fahrräder erscheinen über mehrere Frames hinweg
- Gleiches Fahrrad darf nicht mehrfach gezählt werden
- Objekterkennung allein ist nicht ausreichend

**Lösungsansatz**
- YOLOv8 zur Detektion von Fahrrädern pro Frame
- ByteTrack zur Verknüpfung von Detektionen über mehrere Frames
- Zählung eindeutiger Track-IDs als Näherung der Fahrradanzahl
- Umfassendes Debugging zur Analyse von Fehlerfällen und Limitationen

---

## Ordnerstruktur

eds-ai-git/
├── notebooks/
│ ├── data/
│ │ ├── SCHOE1.mp4
│ │ ├── SCHOE2.mp4
│ │ ├── SCHOE3.mp4
│ │ ├── velopark.mp4
│ │ └── velopark2.mp4
│ │
│ ├── outputs/
│ │ ├── frames/ # Debug-Frames mit Bounding Boxes & Track-IDs
│ │ ├── crops/ # Repräsentative Bildausschnitte pro Track-ID
│ │ ├── annotated.mp4 # Annotiertes Video mit Tracking-IDs
│ │ └── tracks.csv # Aggregierte Track-Tabelle
│ │
│ └── 02_yolo_velo_tracking.ipynb
│
├── yolov8n.pt
└── README.md


> **Hinweis:**  
> Videodateien sind lokal gespeichert und werden nicht versioniert. Große Dateien sind über `.gitignore` ausgeschlossen.

---

## Notebooks

### `02_yolo_velo_tracking.ipynb` 
Dieses Notebook enthält die vollständige Implementierung:

- Laden und Validieren der Videodaten
- Fahrraddetektion mit YOLOv8
- Multi-Object-Tracking mit ByteTrack
- Export eindeutiger Track-IDs
- Automatische Generierung von:
  - Debug-Frames
  - Bildausschnitten pro Fahrrad
  - Annotiertem Video
- Systematische Fehleranalyse (Fragmentierung, Confidence-Verläufe, Verdeckungen)

---

## Methodik

- **Detektion:**  
  YOLOv8 (vortrainiert auf COCO), Klasse *bicycle*

- **Tracking:**  
  ByteTrack zur ID-Zuweisung über mehrere Frames

- **Zählstrategie:**  
  Die geschätzte Anzahl der Fahrräder entspricht der Anzahl eindeutiger Track-IDs im gesamten Video.

---

## Ergebnisse

Die Analyse liefert:
- eine geschätzte Gesamtanzahl an Fahrrädern pro Video
- eine Tabelle aller Track-IDs (`tracks.csv`)
- visuelle Nachvollziehbarkeit durch:
  - annotiertes Video
  - repräsentative Bildausschnitte pro Fahrrad

Die Resultate stellen **Näherungen** dar und keine exakten Wahrheitswerte.

---

## Debugging & Limitationen

Ein zentraler Bestandteil der Arbeit ist das integrierte Debugging:

- Identifikation kurzlebiger Track-IDs
- Analyse niedriger Confidence-Werte
- Visualisierung problematischer Frames
- Untersuchung von ID-Splits bei Verdeckungen

**Typische Limitationen**
- Überlappende Fahrräder
- Teilweise Verdeckungen
- Dichte Abstellanlagen
- Motion Blur bei Kamerabewegung

Diese Limitationen werden explizit analysiert und dokumentiert.

---

## Reproduzierbarkeit

Voraussetzungen:
- Python 3.10+
- OpenCV
- Ultralytics (YOLOv8)
- NumPy, Pandas, Matplotlib

Ausführung:
1. Video unter `notebooks/data/` ablegen
2. Notebook `02_yolo_velo_tracking_gold.ipynb` öffnen
3. Zellen von oben nach unten ausführen

---

## Fazit

Die Arbeit zeigt, dass eine Kombination aus Objekterkennung und Tracking eine praktikable Lösung zur automatischen Fahrradzählung in Vorbeifahr-Videos darstellt.  
Durch das umfassende Debugging bleiben die Resultate transparent und nachvollziehbar, auch in komplexen Szenen.

---



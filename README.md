# RapMap

A graph-based song recommendation system for Desi Hip Hop, built using Breadth First Search (BFS), Depth First Search (DFS), and A* search algorithms.

---

## What is RapMap?

Mainstream platforms like Spotify and Apple Music consistently fail at recommending Desi Hip Hop. Their models are trained on Western music data, Indian artists get mislabeled under Bollywood or World Music, and cultural context — city scenes, collab networks, language nuance — is ignored entirely.

RapMap solves this by modelling songs as nodes in a graph and connecting them based on a custom similarity score built around features that actually matter in this genre: language, vibe, BPM range, city scene, and collaboration history. Three search algorithms then traverse this graph from any seed song to generate recommendations — each algorithm producing a fundamentally different result.

This project was built as part of VITyarthi CSA2001 — Fundamentals in AI and ML Flipped course at VIT Bhopal.

---

## How It Works

### Graph Construction

Each song in the dataset is a node. Two songs are connected by a weighted edge if their similarity score crosses a threshold of 5. The score is computed as follows:

| Feature Match       | Points |
|---------------------|--------|
| Same language       | +3     |
| Same vibe           | +3     |
| Same BPM range      | +2     |
| Same city           | +2     |
| Same artist         | +4     |
| Collaboration       | +4     |

### Search Algorithms

| Algorithm | Strategy                              | Type of Recommendations          |
|-----------|---------------------------------------|----------------------------------|
| BFS       | Level-by-level, strongest edges first | Safe, close, expected picks      |
| DFS       | Deep path, weakest edges first        | Niche, distant, unexpected picks |
| A*        | Heuristic-guided toward target vibe   | Mood-matched, intelligent picks  |

The heuristic used in A* is the target vibe selected by the user. Songs matching the vibe receive a cost of 0. Songs that do not match receive a penalty of 5. A* navigates the graph toward the best vibe match rather than simply the nearest neighbour.

---

## Dataset

The dataset (DHH Database.csv) contains 60 hand-curated Desi Hip Hop songs from 21 artists including Seedhe Maut, Divine, Krsna, Raftaar, MC Stan, Emiway, Prabh Deep, Yashraj, Naam Sujal, Ikka, and more.

Each song has the following fields:

| Field             | Description                              | Example              |
|-------------------|------------------------------------------|----------------------|
| song_id           | Unique identifier                        | 1                    |
| title             | Song title                               | Maina                |
| artist            | Main artist                              | Seedhe Maut          |
| language          | Language(s) used                         | Hindi, Punjabi       |
| vibe              | Mood of the song                         | Aggressive, Conscious|
| bpm_range         | Tempo category                           | slow / mid / fast    |
| city              | City the artist is from                  | Delhi                |
| featured_artists  | Featured artists in the track            | Krsna                |

---

## Project Structure

```
rapmap/
├── rapmap.py          Main program — graph builder, algorithms, interactive menu
├── DHH Database.csv   Hand-curated dataset of 60 Desi Hip Hop songs
└── README.md          This file
```

---

## Requirements

- Python 3.8 or above
- pip (comes bundled with Python 3.4+)

### Dependencies

| Library    | Purpose                                              |
|------------|------------------------------------------------------|
| pandas     | Loading and cleaning the CSV dataset                 |
| networkx   | Building the song graph and traversing it            |
| matplotlib | Generating and saving the graph visualization        |

---

## Setup and Installation

### Step 1 — Clone the repository

```bash
git clone https://github.com/ishan25bai10966/rapmap.git
cd rapmap
```

### Step 2 — Install dependencies

```bash
pip install pandas networkx matplotlib
```

No virtual environment is required. All three libraries are standard and install cleanly via pip.

### Step 3 — Verify file structure

Confirm that both of the following files are present in the same folder:

```
rapmap/
├── rapmap.py
└── DHH Database.csv
```

### Step 4 — Update the CSV path

Open rapmap.py and find line 14:

```python
CSV_FILE = "DHH Database.csv"
```

If your terminal will be running from inside the rapmap folder, no change is needed. If you are running from a different directory, update the path to the full location of DHH Database.csv on your machine.

Windows example:
```python
CSV_FILE = r"C:\Users\YourName\rapmap\DHH Database.csv"
```

Mac or Linux example:
```python
CSV_FILE = "/home/yourname/rapmap/DHH Database.csv"
```

---

## Running the Project

```bash
python rapmap.py
```

The program runs entirely in the terminal. No graphical interface or browser is required.

---

## Using the Program

When the program starts, it loads the dataset and builds the graph. You will then see the following menu:

```
================================================================
  RAPMAP -- Desi Hip Hop Recommender
================================================================

  MAIN MENU
  1. Get song recommendations
  2. Visualize the song graph
  3. Show most connected songs
  4. Exit
```

### Option 1 -- Get song recommendations

A numbered list of all 60 songs is displayed. Enter the number of the song you want to use as a starting point. You will then be asked to select a target vibe and an algorithm.

```
  Select target vibe:
  1. Aggressive
  2. Chill
  3. Conscious
  4. Motivational
  5. Sad

  Select algorithm:
  1. BFS     -- Closest songs first
  2. DFS     -- Deep exploration, niche discoveries
  3. A*      -- Smartest match to your target vibe
  4. Compare -- Run all three and compare results side by side
```

Selecting option 4 (Compare) runs all three algorithms on the same input and prints the results together. This is the most informative output and is recommended for first-time use.

### Option 2 -- Visualize the song graph

Select a seed song. The full similarity graph is generated with nodes colored by city scene and saved as rapmap_graph.png in your project folder. The seed song is highlighted with a black outline.

### Option 3 -- Show most connected songs

Displays the top 5 songs with the most connections in the graph. These are the songs that act as hubs in the Desi Hip Hop ecosystem.

### Option 4 -- Exit

Closes the program.

---

## Sample Output

```
================================================================
  RAPMAP -- Algorithm Comparison
================================================================
  Seed song   : Khatta flow -- Seedhe Maut
  Target vibe : Aggressive
================================================================

  BFS -- Closest songs first (safe picks)
  1. Maina -- Seedhe Maut               [score: 9]  1 hop(s)
  2. Do guna -- Seedhe Maut             [score: 8]  1 hop(s)
  3. Woh raat -- Raftaar                [score: 7]  2 hop(s)
  4. Akatsuki -- Seedhe Maut            [score: 7]  2 hop(s)
  5. Nanchaku -- Seedhe Maut            [score: 7]  2 hop(s)

  DFS -- Deep exploration (niche discoveries)
  1. Blackball -- SOS                   [score: 5]  7 hop(s)
  2. New riot -- Rebel                  [score: 5]  9 hop(s)
  3. The savarkar rage -- Sambata       [score: 5]  11 hop(s)
  4. Naaz -- Siyaahi                    [score: 5]  12 hop(s)
  5. Grind -- Emiway                    [score: 5]  13 hop(s)

  A* -- Smartest match to 'Aggressive' vibe
  1. Woh raat -- Raftaar    [score: 7]  why: Aggressive vibe + fast BPM + Delhi scene
  2. Akatsuki -- Seedhe Maut [score: 8] why: Aggressive vibe + fast BPM + Delhi scene
  3. Jaaluddin -- Gravity   [score: 6]  why: Aggressive vibe + fast BPM + Mumbai scene

  SUMMARY
  BFS  -- Safe, expected picks close to your seed song
  DFS  -- Adventurous, niche, unexpected discoveries
  A*   -- Smartest match targeting 'Aggressive' vibe
```

---

## Updating the Dataset

To add new songs, open DHH Database.csv, add new rows following the existing format, and re-run the program. The graph rebuilds automatically on each run. No code changes are needed.

---

## Limitations

- The dataset contains 60 songs, which is sufficient for demonstration but small by production standards
- Song features are manually tagged, which introduces some subjectivity
- The system does not learn from user behavior -- there is no feedback loop
- Delhi is overrepresented in the dataset, reflecting the real dominance of that scene in Desi Hip Hop

---

## Built With

- Python 3
- NetworkX
- Pandas
- Matplotlib

---

### SUBMITTED BY -                                   
### Name- Ishan Shrivas
### Registration number -25BAI10966

---

## Contact

If you have questions or suggestions, feel free to reach out:
- GitHub: https://github.com/ishan25bai10966
- Email: ishan.25bai10966@vitbhopal.ac.in
  


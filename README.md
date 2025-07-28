# Challenge 1B: Enhanced PDF Analysis Pipeline

## Overview
This project implements a **persona-driven PDF analysis pipeline** designed to parse, analyze, and extract the most relevant portions of PDFs for a given user persona and job-to-be-done. It leverages multi-signal section detection (font size, bolding, layout positioning) combined with semantic analysis powered by pre-trained Sentence Transformer models to output structured JSON summaries tailored to the persona’s needs.

## Features
- Multi-signal PDF section detection based on font size, bold text, layout isolation, and position.
- Robust fallback handling for poorly structured or malformed PDFs.
- Persona-driven semantic analysis using pre-trained Sentence Transformer embeddings.
- Efficient memory and time usage suitable for typical laptop/desktop with ~16GB RAM.
- Fully Dockerized for ease of setup and reproducibility.

## Recommended Directory Structure
```
CHALLENGE_1B/
├── Collection 1/
│   ├── PDFs/
│   │   ├── file1.pdf
│   │   └── ...
│   ├── challenge1b_input.json
│   └── challenge1b_sampleoutput.json
├── Collection 2/
│   ├── PDFs/
│   ├── challenge1b_input.json
│   └── challenge1b_sampleoutput.json
├── Collection 3/
│   ├── PDFs/
│   ├── challenge1b_input.json
│   └── challenge1b_sampleoutput.json
├── main.py
└── requirements.txt
```

## Usage

### Run for a Specific Collection (from project root)
```sh
python main.py --input "Collection 1/challenge1b_input.json" --output "Collection 1/challenge1b_output.json"
```

### Run from Inside a Collection Directory
```sh
cd "Collection 1"
python ../main.py --input challenge1b_input.json --output challenge1b_output.json
```

- The output file **must be named** `challenge1b_output.json` to be accepted by the evaluation system.
- The input JSON should list PDFs relative to itself, e.g., `"PDFs/filename.pdf"`.
- The tool resolves PDF paths relative to the input JSON's location.
- The output JSON is saved to the specified output file path.

## Docker Usage

### Build the Docker image
```sh
docker build -t challenge1b .
```

### Run the pipeline inside the Docker container
```sh
docker run --rm -v $(pwd)/Collection\ 1:/app/collection challenge1b \
    python main.py --input /app/collection/challenge1b_input.json --output /app/collection/challenge1b_output.json
```
Explanation:
- The `-v $(pwd)/Collection\ 1:/app/collection` option mounts your local `Collection 1` folder into the container at `/app/collection`.
- The container runs `main.py` with input and output JSON paths referencing locations inside the container.
- This setup keeps your local data and lets the container write the output back to your local filesystem for easy access.

## Input/Output

- **Input:** JSON workflow file detailing PDF locations (`challenge1b_input.json`).
- **Output:** JSON file containing the extracted and summarized data (`challenge1b_output.json`).
- The output can be compared against the provided `challenge1b_sampleoutput.json` for validation.

## Notes
- Place your PDFs inside each Collection’s `PDFs/` subdirectory.
- The analysis pipeline is designed to be robust to malformed PDFs and parsing errors.
- Use the sample output JSON files provided to validate that your pipeline outputs are correct.

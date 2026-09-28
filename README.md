# Reading Complex and Densely-Annotated Texts
This project investigates how people read and learn from complex texts that are densely annotated and hierarchically organised. It combines eye-tracking, a novel behavioural task, and psychometric measures to understand how readers navigate these texts, what they perceive as structurally important, and how they evaluate them aesthetically.

## Research aims

- Describe how readers navigate densely annotated, hierarchical texts (e.g. moving between main text and annotations)
- Identify which parts of a text readers perceive as important to its organisation
- Examine how navigation patterns and perceived structure relate to learning and to readers' aesthetic evaluation of the text

## Methods

### 1. Eye-tracking
Participants' eye movements are recorded while they read the texts. Fixations, saccades and transitions between text regions (areas of interest) are analysed to characterise navigation patterns.

- Eye tracker: `Mobile Eye-trackers, Neon from Pupil Labs`
- Sampling rate: `200 Hz with a resolution of 192x192px`

### 2. Rectangle-drawing task (perceived importance)
Participants draw rectangles over the parts of the text they perceive as most important to its organisation. The task was developed in JavaScript and embedded in a Qualtrics survey. Rectangle coordinates are saved to Qualtrics embedded data for analysis in JSON.

See [`task/`](task/) for the code and setup instructions.

### 3. Psychometric measures
Questionnaires administered in Qualtrics included 13 features of Aesthetic evaluation Developed by our team (Alejandro et al., 2023).

## Repository structure

```
├── task/                               # JavaScript rectangle-drawing task for Qualtrics
├── stimuli/                            # Texts and annotation layouts
├── data/                               # Raw and processed data (see Data section)
├── pre-processing_and_analysis/        # Analysis scripts (Python / R)
└── results/                            # Figures and model outputs
```

## Using the rectangle-drawing task in Qualtrics

1. **Add the HTML.** Create a Text/Graphic question in Qualtrics, open the question text's **HTML View**, and paste in the code from [`html/`](html/).
2. **Link your image.** Upload the text image to your Qualtrics Library, copy its URL, and replace the image link in the HTML with it.
3. **Add the JavaScript.** Open the question's **JavaScript** editor and paste in the code from [`task/`](task/).
4. **Set up data storage.** In **Survey Flow**, add an **Embedded Data** element *before* the question block, and create the fields used to store the rectangle data (e.g. `[field_name]`).
5. **Test it.** Preview the survey, draw a few rectangles, then export the responses and check that the coordinates are recorded.

## Analysis

Work in progress

## Data

The data will be available here soon

## Contact

Mirza Muchammad Iqbal, University of Glasgow
mirzamuchammad.iqbal@glasgow.ac.uk


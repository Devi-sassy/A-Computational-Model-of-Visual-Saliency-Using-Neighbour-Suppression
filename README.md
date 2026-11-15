# A Computational Model of Visual Saliency Using Neighbour Suppression

## Overview

This is a self-directed project that explores visual saliency through a simplified computational model inspired by the idea of feature-based suppression in the primary visual cortex (V1).

The model investigates why an object that differs from its surrounding elements can stand out visually. Specifically, it simulates a visual display containing one tilted bar among vertically oriented bars and predicts the location of the most salient element.

The project uses Python and NumPy to generate orientation grids, calculate similarity between neighbouring elements, estimate suppression, and construct a saliency map.

## Objectives

- Understand how local feature similarity can influence visual saliency.
- Implement a neighbour-based suppression mechanism.
- Investigate how orientation difference affects target detection.
- Examine how the similarity parameter (sigma) influences suppression and detection.
- Compare total suppression with average suppression to investigate the effect of neighbourhood size.
- Study how response noise affects model performance.

## Methodology

### 1. Generating the Orientation Grid

A 10 × 10 grid is generated in which all elements initially have a vertical orientation of 0°. One randomly selected element is assigned a different orientation to represent the target.

Gaussian noise is added to the orientations to introduce small variations in the display.

### 2. Calculating Similarity

Similarity between two orientations is calculated using a Gaussian function:

$$
S(a,b)=\exp\left(-\frac{(a-b)^2}{2\sigma^2}\right)
$$

Where:
- \(a\) and \(b\) represent two orientations.
- \(\sigma\) controls the width of the similarity function.
- Higher similarity indicates that two orientations are more alike.

### 3. Calculating Neighbour Suppression

The model considers the eight surrounding neighbours of each element in a 3 × 3 local neighbourhood, excluding the element itself.

Initially, suppression was calculated by adding the similarity values of all available neighbours.

Later, the calculation was modified to use average suppression:

$$
\text{Average Suppression}=
\frac{\text{Total Similarity}}{\text{Number of Neighbours}}
$$

This modification was introduced to account for differences in the number of available neighbours at corner, edge, and middle positions.

### 4. Constructing the Saliency Map

The saliency of each element is calculated using:

$$
\text{Saliency}=
\frac{\text{Response}}{1+\text{Suppression}}
$$

Elements with lower suppression receive higher saliency values.

The model predicts the target location by selecting the position with the maximum saliency value.

## Experiments

### Experiment 1: Effect of Orientation Difference

The orientation difference between the target and surrounding elements was varied from 0° to 90° in increments of 5°.

- Similarity width: sigma = 15
- Trials per orientation difference: 200
- Detection accuracy was calculated as the percentage of correct predictions.

**Observation:** Detection accuracy increased as the orientation difference increased. When the target was similar to its neighbours, detection was difficult. As the orientation difference increased, the target generally became easier for the model to identify.

### Experiment 2: Effect of Sigma

The experiment was repeated using three different sigma values:

- Sigma = 5
- Sigma = 15
- Sigma = 30

The same generated displays were used across sigma conditions to make the comparison consistent.

**Observation:** The detection accuracy curves were almost identical under the constant-response conditions tested. Although sigma changed the similarity and suppression values, it did not noticeably change the predicted target locations in these experiments.

### Experiment 3: Effect of Response Noise

Response noise was introduced to investigate how variations in the response values influence detection performance.

- Sigma values: 5, 15, and 30
- Trials per orientation difference: 1,000
- Response values included Gaussian noise, with a minimum value of 0.01.

**Observation:** Under the tested response-noise conditions, sigma = 5 produced the highest detection accuracy, followed by sigma = 15 and sigma = 30.

This suggests that the choice of similarity width can influence detection performance when response noise is introduced.

## Additional Analysis: Total vs Average Neighbour Suppression

A controlled comparison was performed using a 10 × 10 grid in which every element had the same orientation.

The number of available neighbours differs according to position:

| Position | Number of neighbours |
|---|---:|
| Corner | 3 |
| Edge | 5 |
| Middle | 8 |

Since all orientations were identical, every similarity value was 1.

Therefore, total suppression was different at each position:

- Corner: 3
- Edge: 5
- Middle: 8

However, after dividing by the number of available neighbours, average suppression was 1 at all three positions.

**Observation:** Average suppression removes the difference caused purely by the number of available neighbours in this controlled example. This helps prevent the model from assigning different suppression values solely because an element is located at a corner, edge, or middle position.

## Technologies Used

- Python
- NumPy
- Matplotlib

## How to Run

1. Clone or download this repository.
2. Install the required libraries:

   ```bash
   pip install numpy matplotlib
   ```

3. Open the Jupyter Notebook.
4. Run the notebook cells from top to bottom to generate the orientation grids, suppression maps, saliency maps, and experimental graphs.

## Limitations

- This is a simplified computational model, not a complete model of human visual perception.
- The model uses a limited 3 × 3 neighbourhood for suppression.
- The current similarity calculation uses direct orientation subtraction and does not yet account for circular orientation differences.
- Response noise is synthetically generated and does not represent measured biological neural responses.
- The model predicts saliency using the maximum value in a saliency map; it does not simulate eye movements or saccades.

## Project Status

Ongoing self-directed project, started in October 2026.

The project is being developed to explore computational approaches to visual saliency and the relationship between local feature similarity and target detection.

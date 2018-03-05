# Model Proposal for _Farmer decision making in the face of climate change_

_Harrison Smith_

* Course ID: CMPLXSYS 530,
* Course Title: Computer Modeling of Complex Systems
* Term: Winter, 2018



&nbsp; 

### Goal 
*****
 
The goal of this model will be to explore how farmers in different contexts may respond or adapt to climate change. Specifically, it will focus on how individual perceptions of climate change may affect sow dates and the agricultural matrix at the landscape level. 

&nbsp;  
### Justification
****
Farmer decision is a complex and heterogeneous process that is highly context dependent. It is affected both by individual and environmental variables, and is therefore difficult to model using other approaches. ABM is well suited for this problem because it is capable of incorporating heterogeneity at the farmer level and can therefore offer a more holistic, bottom-up explanatory model of landscape level farming patterns. 

&nbsp; 
### Main Micro-level Processes and Macro-level Dynamics of Interest
****

Farmer decision making at the individual level is dependent on many factors, such as income, irrigation type, market access, and farm size, among others. At the Micro-level, this model will focus on the process of farmers deciding what date to sow their crops. If the first iteration of this is successful, I would eventually like to incorporate other factors such as wealth of farmers, irrigation type, market access, and different farm sizes. At the Macro-level, this model will examine the dynamics of how crop yields are affected by a changing climate, and if or how farmers are able to respond to these changes. The emergent agricultural matrix that results from heterogeneous farmer decision making will also be the subject of analysis.


&nbsp; 
## Model Outline
****
&nbsp; 
### 1) Environment
The space of the model will consist of a 2D grid where each cell represents a farmer's plot. Each agent (farmer) will be located on a single cell and will interact only with that cell and the global environment.

Environment-owned variables
* Temperature (The rate of temperature increase per year, ranging from 0 to 0.1)
* Monsoon Season (True or false value, true for only for 150 because monsoon season is ~5 months)
* Water (The amount of water currently located on each grid cell, ranging from 0 to 1)
* Drain rate (a randomly generated rate of drainage of water from each cell) 
* Yield (The amount of crop biomass currently located on each grid cell, ranging from 0 to 10)


Environment-owned methods/procedures  
* Initialize (random drain rate seed in all grid cells)
* Drain (water is depleted from each cell by fixed amount per step)
* Evaporate (water evaporates as a function of temperature)
* Rain (replenishes water in grid cells. Amount and frequency of rain is dependent on if Monsoon season = T or F)
* Start Monsoon (set Monsoon = T, dependent on temperature where higher temperature delays monsoon start)
* End Monsoon (set Monsoon = F after 175 steps)
* Grow (crops grow if the cell has been sown and water content > 0.25 Growth rate is a function of water and continues for 120 steps)
* Die (crops die if water < 0.25 for too long)

```python
# Include first pass of the code you are thinking of using to construct your environment
# This may be a set of "patches-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any patch methods/procedures you have. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block
```

&nbsp; 

### 2) Agents
 
The agents in this model will be individual farmers, which are each associated with a given grid cell.

Agent-owned variables
* Memory (a recollection of when the monsoon came in previous years)
* Yield history (a history of all yields from years past)

Agent-owned methods/procedures
* Sow (farmer decides to sow crops based on a predetermined baseline or memory if available)
* Reap (farmer harvests yield on cell and sets cell yield value to zero. The harvest will be converted to wealth in later iterations)

```python
# Include first pass of the code you are thinking of using to construct your agents
# This may be a set of "turtle-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any agent methods/procedures you have so far. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block
```

&nbsp; 

### 3) Action and Interaction 
 
**_Interaction Topology_**

In the first round, agents will only interact with their own grid cell or 'plot'. They will decide when to sow based on a baseline start time for the monsoon season, and will harvest their crops when the growing season is over. In subsequent rounds, agents will use their memory to inform a prediction of when the monsoon season will occur, and will sow their crops accordingly. In the first iteration of this model, there will be no interaction between agents. 
 
**_Action Sequence_**

_What does an agent, cell, etc. do on a given turn? Provide a step-by-step description of what happens on a given turn for each part of your model_

1. Step 1
2. Step 2
3. Etc...

&nbsp; 
### 4) Model Parameters and Initialization

_Describe and list any global parameters you will be applying in your model._

_Describe how your model will be initialized_

_Provide a high level, step-by-step description of your schedule during each "tick" of the model_

&nbsp; 

### 5) Assessment and Outcome Measures

_What quantitative metrics and/or qualitative features will you use to assess your model outcomes?_

&nbsp; 

### 6) Parameter Sweep

_What parameters are you most interested in sweeping through? What value ranges do you expect to look at for your analysis?_

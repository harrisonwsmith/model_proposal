# Model Proposal for _Farmer decision making and climate change_

_Harrison Smith_

* Course ID: CMPLXSYS 530,
* Course Title: Computer Modeling of Complex Systems
* Term: Winter, 2018



&nbsp; 

### Goal 
*****
 
The goal of this model will be to explore how small-holder farmers in different contexts may respond or adapt to climate change. Specifically, it will focus on how individual perceptions of climate change may affect sow dates at the landscape level. 

&nbsp;  
### Justification
****
Farmer decision is a complex and heterogeneous process that is highly context dependent. It is affected both by individual and environmental variables, and is therefore difficult to model using other approaches. ABM is well suited for this problem because it is capable of incorporating heterogeneity at the farmer level and can therefore offer a more holistic, bottom-up explanatory model of landscape level farming patterns. 

&nbsp; 
### Main Micro-level Processes and Macro-level Dynamics of Interest
****

Farmer decision making at the individual level is dependent on many factors, such as income, irrigation type, market access, and farm size, among others. At the Micro-level, this model will focus on the process of farmers deciding what date to sow their crops. If the first iteration of this is successful, I would eventually like to incorporate other factors such as wealth of farmers, irrigation type, market access, and different farm sizes. At the Macro-level, this model will examine the dynamics of how crop yields are affected by a changing climate, and how and when farmers are able to respond to these changes. The emergent agricultural matrix that results from heterogeneous farmer decision making will also be the subject of analysis.


&nbsp; 
## Model Outline
****
&nbsp; 
### 1) Environment
_Description of the environment in your model. Things to specify *if they apply*:_

* _Boundary conditions (e.g. wrapping, infinite, etc.)_
* _Dimensionality (e.g. 1D, 2D, etc.)_
* _List of environment-owned variables (e.g. resources, states, roughness)_
* _List of environment-owned methods/procedures (e.g. resource production, state change, etc.)_


```python
# Include first pass of the code you are thinking of using to construct your environment
# This may be a set of "patches-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any patch methods/procedures you have. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block
```

&nbsp; 

### 2) Agents
 
 _Description of the "agents" in the system. Things to specify *if they apply*:_
 
* _List of agent-owned variables (e.g. age, heading, ID, etc.)_
* _List of agent-owned methods/procedures (e.g. move, consume, reproduce, die, etc.)_


```python
# Include first pass of the code you are thinking of using to construct your agents
# This may be a set of "turtle-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any agent methods/procedures you have so far. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block
```

&nbsp; 

### 3) Action and Interaction 
 
**_Interaction Topology_**

_Description of the topology of who interacts with whom in the system. Perfectly mixed? Spatial proximity? Along a network? CA neighborhood?_
 
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

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
* Temperature (average temperature for the season)
* Temperature rate of change (The rate of temperature increase per year, ranging from 0 to 0.1)
* Monsoon Season (True or false value, true for only for 50 steps)
* Water (The amount of water currently located on each grid cell, ranging from 0 to 1)
* Drain rate (a randomly generated rate of drainage of water from each cell) 
* Yield (The amount of crop biomass currently located on each grid cell)


Environment-owned methods/procedures  
* Initialize (random drain rate seed in all grid cells)
* Drain (water is depleted from each cell by fixed amount per step)
* Evaporate (water evaporates as a function of temperature)
* Rain probability (probability of rainfall, will be higher probability during monsoon)
* Rain (replenishes water in grid cells. Amount of rain is dependent on if Monsoon season = T or F)
* Start Monsoon (set Monsoon = T, dependent on temperature where higher temperature delays monsoon start)
* End Monsoon (set Monsoon = F after 50 steps)
* Grow (crops grow if the cell has been sown and water content > 0.2 Growth rate is a function of water and continues for 40 steps)
* Die (crops die if water < 0.2 for too long)

```python
# Include first pass of the code you are thinking of using to construct your environment
# This may be a set of "patches-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any patch methods/procedures you have. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block

# Imports
import numpy
import matplotlib.pyplot as plt
import pandas

# Import widget methods
from ipywidgets import interact

class Model:
    def __init__(self, grid_size, temp_init, temp_change, water, yield, drain_rate, evaporate, rainprob, rain, monsoon_init, monsoon_new, grow, die):
        # set up model parameters
        self.grid_size = grid_size
        self.temp_init = temp_init
        self.temp_change = temp_change
        # define defaults for above three parameters
        self.evaporate = 
        # a constant * temperature
        self.rainprob = 
        # probability of rain event (0.25 when monsoon=F, 0.5 when monsoon=T)
        self.rain = 
        # fill every cell with rain (0-0.25 rain when monsoon=F, 0.25 to 0.5 when monsoon=T)
        self.monsoon_init = 
        # if t=10, set True, ends at t=60
        self.monsoon_new = 
        # update monsoon date based on temperature
        self.grow = 
        # grow crops if seeded and if water
        self.die = 
        # kill crops if no water or temp too high
        
        self.drain_rate = 
        # randomly distribute drainage rates among cells
        self.water = 
        # amount of water in a given cell. Maybe this belongs in the cell class?
        self.yield = 
        # amoung of crops in a given cell. Maybe this belongs in the cell class also?


        # set state variables
        self.t = 0
        self.space = numpy.array((0,0)) 
        # this needs to be a 3D array I think...
        self.farmers = []
        self.num_seasons = 0
        self.num_crop_failures = 0
        
        
        # set up history variables
        self.history_space = []
        self.history_yield_global = 
        self.history_yield_local = 
        # unsure of how to do "local" or cell by cell yeild value
        self.history_water = []
        # this also needs to be local, or cell by cell
        self.history_monsoon_start = []
        # this will be global
        
        # Call setup methods to initialize cells, farmers, and environment
        self.setup_cells()
        self.setup_farmers
        self.setup_environment
        
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

In a given turn... 

The environment will:
1. Set evaporation rates as a function of current temperature
2. Test probability of rain, if True then rain on all cells

Each cell will:
1. Change the amount of water in each cell based on evaporation rate and drainage rate
2. Grow crops if there is enough water, the cell is sown, and it is during growing season (within 50 days of sow date)

Each agent will:
1. Sow crops if not sown yet and store the sow date
2. Record total yield and then set yield to zero if date = (sow date) + 50



&nbsp; 
### 4) Model Parameters and Initialization

The global parameters that will be used in this model will be temperature, evaporation rate, growth rate, and monsoon season.

The model will be initialized with no crops and a start time (t) of zero. Drainage rates will be randomly distributed throughout the grid using a specified seed, and evaporation rate will be calculated based on the temperature. Initial rain probability and amount will be set for the non-monsoon season, and an initial rain event will add some water to all patches. Farmers will sow their crops using a baseline (the "historical" start date of the monsoon) when t=5, or after 5 steps. In the first round or "season", the monsoon will start on this baseline date (t=5). After cells have been sown, crops will grow as a function of water availability (sigmoid function).

The model schedule will then procede as follows:
1. Model is initialized using above description
2. Farmers sow crops using baseline (t=10) or average of 5 previous monsoon start dates
3. Monsoon starts at t=10 intially, but is delayed as a function of temperature increase rate
4. If a cell has sufficient water, plants grow (growth rate is a sigmoid function where x=water amount)
5. After 40 steps from sow date, crops stop growing
6. After 50 steps, the monsoon season ends
7. Farmer's harvest crops
8. Adjust monsoon start date based on temperature rate change
9. Run model until monsoon season ends
10. Restart the growing season (t=0)
11. Farmer's calculate new sow date using previous monsoon start dates
12. Update temperature using rate of temperature change
13. Update evaporation rate using new temperature
12. Update Monsoon start date using temperature
13. Repeat above steps for specified number of "seasons"

&nbsp; 

### 5) Assessment and Outcome Measures

In order to assess my model outcomes, I will use several metrics. The main metric I will look at will be sow date over time, which will answer my central question of how sow date will change in response to perception of climate change. I will also look at overall yield for each pixel and how that corresponds to sow date, and I will use this to determine the adaptive capacity of farmers in the face of climate change. 

In future iterations, I would also like to incorporate other factors such as income and wealth generated from crops. If I am able to incorporate these factors, I will also gather data on changes in wealth in order to assess my model outcomes.

&nbsp; 

### 6) Parameter Sweep

The main parameter sweep I would look at is how different rates of temperature change affect farmer's ability grow crops and how it affects their sow date. I think it will also be interesting to look at different amounts of rainfall and how this might affect the results I get. 

In future iterations, I also hope to look at how different ranges of wealth, irrigation, or access to markets might affect my results. These parameters would also be included in a parameter sweep. 

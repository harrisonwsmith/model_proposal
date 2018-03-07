# Model Proposal for _Farmer decision making in the face of climate change_

_Harrison Smith_

* Course ID: CMPLXSYS 530,
* Course Title: Computer Modeling of Complex Systems
* Term: Winter, 2018



&nbsp; 

### Goal 
*****
 
The goal of this model will be to explore how farmers in India may respond or adapt to increased variability in the onset of the monsoon season. Specifically, it will focus on how access to irrigation during the monsoon season may affect sow dates, irrigation use, and and yield. 

&nbsp;  
### Justification
****
Farmer decision is a complex and heterogeneous process that is highly context dependent. It is affected both by individual and environmental variables, and is therefore difficult to model using other approaches. ABM is well suited for this problem because it is capable of incorporating heterogeneity at the farmer level and can therefore offer a more holistic, bottom-up explanatory model of landscape level farming patterns. 

&nbsp; 
### Main Micro-level Processes and Macro-level Dynamics of Interest
****

Farmer decision making at the individual level is dependent on many factors, such as income, irrigation, market access, and farm size, among others. At the Micro-level, this model will focus on the process of farmers deciding when to sow their monsoon season crops. The sow date of monsoon crops is dependent upon access to irrigation. Farmers with access to irrigation may sow their crops earlier in the event of a delayed monsoon, while the sow date of farmers who do not have access to irrigation is dependent upon the arrival of the monsoon rains. If the first iteration of this is successful, I would eventually like to incorporate other factors such as wealth of farmers, market access, and different farm sizes. At the Macro-level, this model will examine how often farmers irrigate their crops and how much yield farmers 


&nbsp; 
## Model Outline
****
&nbsp; 
### 1) Environment
The space of the model will consist of a 2D grid where each cell represents a farmer's plot. Each agent (farmer) will be located on a single cell and will interact only with that cell and the global environment.

Environment-owned variables
* Monsoon start (date of start of monsoon)
* yield_total (The total crop biomass currently located on every grid cell)
* yield_low (The mean crop biomass of farmers with low access to irrigation)---> only irrigate in winter, 2-6 times in winter
* yield_high (The total crop biomass of farmers with high access to irrigation)--> Irrigate for late monsoon and 2-6 times in winter


Environment-owned methods/procedures  
* Initialize (random drain rate seed in all grid cells)
* 
* Start Monsoon (set Monsoon = T, dependent on temperature where higher temperature delays monsoon start)
* End Monsoon (set Monsoon = F after 50 steps)
* Grow (crops grow if the cell has been sown and water content > 0.2 Growth rate is a function of water and continues for 40 steps)
* Die (crops die if water < 0.2 for too long)

```python

# Imports
import numpy
import matplotlib.pyplot as plt
import pandas

# Import widget methods
from ipywidgets import interact

class Model: # why write (Object) after the class name?
    def __init__(self, grid_size, monsoon_start=152, water, yield, drain_rate, pressure, evaporate, rainprob, rain, monsoon_init, monsoon_new, grow, die):
        # set up model parameters
        self.grid_size = grid_size
        self.monsoon_start = monsoon_start
        self.monsoon_init = # if t=10, set True, ends at t=60
        self.monsoon_new = # update monsoon date based on temperature
        self.yield = # amoung of crops in a given cell. Maybe this belongs in the cell class also?


        # set state variables
        self.t = 0
        self.space = numpy.array((0,0)) # this needs to be able to reference a 3D array I think
        self.farmers = []
        self.num_years = 0
        
        # set up history variables
        self.history_space = [] # not sure yet if I need this one
        self.history_yield_global = []
        self.history_monsoon_start = []
        
        # Call setup methods to initialize cells, farmers, and environment
        self.setup_cells()
        self.setup_farmers()
        self.setup_environment() # I might need more setup functions but can't think of any right now
        
    def setup_cells(self):
        """
        Method to setup cells
        """
        # fill this in
        
    def setup_farmers(self):
        """
        Method to setup farmers
        """
        # fill this in
        
    def setup_environment(self):
        """
        Method to setup environment
        """
        # fill this in
    
    def get_farmer_position(self, famer_id):
        # will need to define farmer_id in the setup_farmers fucntion
    
    def rain(self, rainprob):
        # not sure about this
        
    def grow(self, # if seeded, # if enough water):
    
    def get_yield_global(self):
    
    def get_yield_local(self, farmer_id):
    
    def get_water(self, farmer_id):
    
    def get_sow_date(self, farmer_id):
    
    def step(self):
    
    def show_yield(self, t=None):
    """
    Return a projection of the space that shows how much crop yield is on each cell
    """
    
    def show_water(self, t=None):
    """
    Return a projection of the space that shows how much water is on each cell
    """
    
    
    
```

&nbsp; 

### 2) Agents
 
The agents in this model will be individual farmers, which are each associated with a given grid cell.

Agent-owned variables
* Yield history (a history of all yields from years past)
* Irrigation history (number of times they irrigated their plot)

Agent-owned methods/procedures
* sow_monsoon (farmer decides to sow crops based on a predetermined baseline or memory if available)
* harvest_monsoon
* irrigate (farmer irrigates their plot)
* sow_winter
* harvest_winter

```python
# Setup a class for the farmers
class Farmer:
    def __init__(self, model, farmer_id, irrigation_access, sow_date, harvest_date, irrigation_count)
    """
    farmer by default sows on the established baseline and harvest x days after
    """
    # set model link and farmer id
    self.model = model
    self.farmer_id = farmer_id
    
    # set farmer parameters
    self.sow_monsoon = 
        if irrigation == TRUE:
            sow_date = 152
        else:
            sow_monsoon = monsoon_start
    self.harvest_monsoon = sow_monsoon + 122
    self.sow_winter = harvest_monsoon + 45
    self.harvest_winter = sow_winter + 122
    
    
    # set farmer history
    self.monsoon_sowdate_history = []
    self.monsoon_irrgation_count = 0
    self.winter_irrigation_count = 0
    self.total_irrigation_count = 0
    self.yield_history_local = 


    
    def sow(self, sow_date):
    
    
    def harvest(self, harvest_date):

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

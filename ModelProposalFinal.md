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

Farmer decision making at the individual level is dependent on many factors, such as income, irrigation, market access, and farm size, among others. At the Micro-level, this model will focus on the process of farmers deciding whether or not to irrigate their monsoon season crops. The sow date of monsoon crops is dependent upon access to irrigation. Farmers with access to irrigation may sow their crops earlier in the event of a delayed monsoon, while the sow date of farmers who do not have access to irrigation is dependent upon the arrival of the monsoon rains. This becomes important because if monsoon cropping is delayed, winter crops will also be delayed. Winter crops (wheat) are exposed to heat stress and at the end of the growing season as the summer months approach, and therefore they suffer yield loss proportional to the number of days of delay. At the Macro-level, this model will examine how often farmers irrigate their crops and how much yield farmers get based on different irrigation practices. If the first iteration of this is successful, I would  like to incorporate perceptions of climate change into the model, may have an effect on irrigation decisions. This could have important implications for ground water depletion, as farmers may choose to irrigate monsoon crops in order to adapt to climate change, further exploiting an already overexploited resource. 

&nbsp; 
## Model Outline
****
&nbsp; 
### 1) Environment
The space of the model will consist of a 2D grid where each cell represents a farmer's plot. Each agent (farmer) will be located on a single cell and will interact only with that cell and the global environment.

Environment-owned variables
* Monsoon delay (how many days the monsoon is delayed)
* global yield (yield of all cells combined)
* yield_nonirrigated (The total yield of farmers without access to irrigation during the monsoon season)
* yield_irrigated (The total yield of farmers with access to irrigation during monsoon season)


Environment-owned methods/procedures  
* Setup (sets up the space and the farmers)
* Get farmer position (returns position of a given farmer)
* Get yield of non irrigated plots
* Get yield of irrigated plots
* Get total yield from all plots
* Step (each step represents a year)
* Show yield (display a grid showing yield amounts on each cell)


```python

# Imports
import numpy
import matplotlib.pyplot as plt
import pandas

# Import widget methods
from ipywidgets import interact

class Model: # why write (Object) after the class name?
    def __init__(self, grid_size, monsoon_delay=0, global_yield=0):
        # set up model parameters
        self.grid_size = grid_size
        self.monsoon_delay = monsoon_delay
        self.global_yield = global_yield

        # set state variables
        self.t = 0
        self.space = numpy.array((0,0)) # this needs to be able to reference a 3D array I think
        self.num_farmers = ((self.grid_size)*(self.grid_size))
        self.farmers = []
        
        # set up history variables
        # self.history_monsoon_delay = [] might add this in when I figure out how to parameterize variation in monsoon delay
        self.history_global_yield = []
        self.history_global_irrigation = []
        
        # Call setup methods to initialize cells, farmers, and environment
        self.setup_plots()
        self.setup_farmers() # I might need more setup functions but can't think of any right now
        
    def setup_plots(self):
        """
        Method to setup gridcells
        """
        # Initialize a space with Nan's
        self.space = numpy.full((self.grid_size, self.grid_size), numpy.nan)
        
    def setup_farmers(self):
        """
        Method to setup farmers
        """
        # create all farmers without placing them
        for i in xrange(self.num_farmers):
            self.farmers.append(Person(model=self,
                                       farmer_id=i,
                                       irrigation_access=False))
        # place farmers into the space
        for farmer in self.farmers:
            # loop until unique
            is_occupied = True
            while is_occupied:
                # Sample location... Maybe this doesn't need to be random. Try range()?
                random_x = numpy.random.randint(0, self.grid_size)
                random_y = numpy.random.randint(0, self.grid_size)
                
                # check if unique
                if numpy.isnan(self.space[random_x, random_y]):
                    is_occupied = False
                else:
                    is_occupied = True
                    
            # place farmer by setting their ID        
            self.space[random_x, random_y] = farmer.farmer_id
            
            # give half of farmers access to irrigation during monsoon
            if farmer.farmer_id >= (0.5 * self.num_cells):
                farmer.irrigation_access=True
        

    def get_farmer_position(self, farmer_id):
        return numpy.reshape(numpy.where(self.space == farmer_id), (1,2))[0].tolist()

    def get_global_yield(self):
        yield_list = []
        for farmer in self.farmers:
            yield_list.append(farmer.farmer_yield)
        return fsum(yield_list)
        
    def get_global_irrigation(self):
        irrigation_list = []
        for farmer in self.farmers:
            irrigation_list.append(farmer.farmer_total_irrigation)
        return sum(irrigation_list)
    
    def step_year(self):
        self.farmers.monsoon_season()
        self.farmers.winter_season()
        self.farmers.get_farmer_position()
    
    def step(self):
        self.setup_plots()
        self.setup_farmers()
        
        self.history_global_yield.append(self.get_global_yield)
        self.history_global_irrigation.append(self.get_global_irrigation)
        self.farmers.reset()
        self.t += 1
        
    def show_yield(self, t=None):
    """
    Return a projection of the space that shows how much crop yield is on each cell
    """
    
    # haven't gotten to this yet
  
```

&nbsp; 

### 2) Agents
 
The agents in this model will be individual farmers, which are each associated with a given grid cell.

Agent-owned variables
* Farmer ID (unique id for each farmer)
* Irrigation access (whether or not a farmer has access to irrigation during monsoon season)
* Yield history (a history of all yields from years past)
* Irrigation history (number of times they irrigated their plot, and what season they did it)
* Irrigation access (access to irrigation during monsoon season)

Agent-owned methods/procedures
* monsoon season (farmer decides to irrigate or not based on if monsoon is delayed)
* winter season (farmer irrigates plot random number of times in winter season, and yield is calculated based on if farmer delayed monsoon crop)
* get farmer ID (gets the ID for a given farmer)


```python
# Setup a class for the farmers
class Farmer:
    def __init__(self, model, farmer_id, irrigation_access=False)
        """
        farmer by default sows on the established baseline and harvest x days after
        """
        # set model link and farmer id
        self.model = model
        self.farmer_id = farmer_id
    
        # set farmer parameters
        self.irrigation_access = irrigation_access
    
        # set farmer state variables
        self.mon_irrgation_count = 0
        self.win_irrigation_count = 0
        self.farmer_total_irrigation = (self.win_irrigation_count + self.mon_irrigation_count)
        self.farmer_yield = 0
        
        # set farmer history
        self.history_farmer_yield = []
        self.history_mon_irrigation = []
        self.history_win_irrigation = []
        self.hisory_farmer_total_irrigation = []
        
    # farmers decide to irrigate their crops if monsoon is delayed
    def monsoon_season(self):
        # if monsoon is not delayed, all farmers will plant crops at same time (monsoon start date)
        if self.model.monsoon_delay == 0:
            self.mon_irrigation_count = 0 
        else: 
            # if monsoon is delayed, farmers with irrigation access will irrigate so they can go ahead and plant their monsoon crop
            if self.model.monsoon_delay =< 15
                if self.irrigation_access == True:
                    self.mon_irrigation_count = 1
            # if monsoon is delayed more than 15 days, farmers with access to irrigation may irrigate again
            if self.model.monsoon_delay > 15
                if self.irrigation_access == True:
                    self.mon_irrigation_count = 2 
        self.history_mon_irrigation.append(mon_irrigation_count)
    
    # farmers irrigate crops during winter in a heterogeneous fashion
    def winter_season(self):
        self.win_irrigation_count = numpy.random.randint(2,6)
        
        # if farmers did not irrigate during monsoon season, their winter crops will suffer heat stress
        if self.model.monsoon_delay > 0:
            if self.irrigation_access == False:
                self.farmer_yield = 1-(self.monsoon_delay/100)
        else:
            self.farmer_yield = 1
        
        # update the irrigation history lists for each farmer
        self.history_win_irrigation.append(win_irrigation_count)
        self.history_farmer_total_irrigation.append(farmer_total_irrigation_count)
        self.history_farmer_yield.append(self.farmer_yield)
        
    # I'm not exactly sure what this is doing at this point
    def get_position(self):
        return self.model.get_farmer_position(self.farmer_id)
        
    def reset(self):
        self.mon_irrgation_count = 0
        self.win_irrigation_count = 0
        self.farmer_yield = 0
```

&nbsp; 

### 3) Action and Interaction 
 
**_Interaction Topology_**

In the first round, agents will only interact with their own grid cell or 'plot' and their environment. They will decide whether or not to irrigate their crops based on the start time for the monsoon season and their access to irrigation, and their yields will be the result of if they had to delay their monsoon crop. In the first iteration of this model, there will be no interaction between agents, but social dynamics may be incorporated in later iterations.
 
**_Action Sequence_**

In a given turn... 

The environment will:
1. Set cells up with one farmer in each cell
2. Calculate global yield
3. Calculate global irrigation count

Each agent will:
1. Decide to irrigate during monsoon season or not based on access to irrigation
2. Irrigate winter crops
2. Record number of instances of irrigation and record yield



&nbsp; 
### 4) Model Parameters and Initialization

The global parameters that will be used in this model will be monsoon delay.

The model will be initialized with a start time (t) of zero. 

The model schedule will then procede as follows:
1. Model is initialized using above description
2. Farmers decide whether to irrigate during monsoon season
3. Farmers irrigate heterogeneously durring winter season
4. Farmers calculate yield and record irrigation counts
5. Farmers store irrigation counts and yield in history
6. Global environment calculates global yield and irrigation counts
7. Farmers reset yield and irrigation counts
8. Repeat model for a new season

&nbsp; 

### 5) Assessment and Outcome Measures

In order to assess my model outcomes, I will look at number of irrigation instances over time, and yield over time, both at the global and individual level. I will also compare yield differences between farmers who have access to irrigation during the monsoon season with farmers who do not.

In future iterations, I would also like to incorporate other factors such as wealth, climate change perception, and ground water depletion. If I am able to incorporate these factors, I will also gather data on changes in wealth in order to assess my model outcomes. In addition, if I can incorporate ground water depletion and climate change perception, I will also include these in my analysis.

&nbsp; 

### 6) Parameter Sweep

The main parameter sweep I would look at is how variability in monsoon date will affect farmer's yield and how it affects their frequency of irrigation. 

In future iterations, I also hope to look at how different ranges of wealth, irrigation, or access to markets might affect my results. These parameters would also be included in a parameter sweep for future iterations. 

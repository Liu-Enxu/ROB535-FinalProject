# ROB535-FinalProject 
  ## Abstract 
  The increasing development of autonomous systems, particularly self-driving cars, demands robust and adaptive approaches to dynamic path planning. This project explores the integration of Markov Decision Processes (MDPs) and Model Predictive Control (MPC) to address the challenges of real-time decision-making and navigation under uncertainty. By leveraging the probabilistic state-transition modeling of MDPs within a Bird’s Eye View (BEV) graph representation, and the optimization capabilities of MPC, this work aims to enhance global planning, and local trajectory tracking. The proposed approach aspires to advance the development of safer, more reliable autonomous driving systems while maintaining computational efficiency. The outcomes of this study hold promise for practical implementation in real-world autonomous applications, contributing to the broader field of intelligent transportation solutions. 
  ## 2D Grid Map Construction 
  This world2d Python class allows for the construction, visualization, and interaction with a customizable 2D grid world. The grid map can represent obstacles, a start point, and an endpoint for use in MDP path-planning algorithms. 
  ### Features 
  Interactive Obstacle Placement: Use a graphical interface to add or remove obstacles on the grid. 
  Customizable Grid Size: Modify the height, width, and cell size of the grid. 
  Start and End Point Selection: Select starting and ending positions interactively. 
  Visualization: Render the grid world with visual feedback using pygame. 
  Obstacle Hollowing: Automatically generate hollow obstacles from binary maps using morphological erosion. 
### Key Methods

```
add_fence(grid_y, grid_x)
```
Adds an obstacle to the grid at the specified row (grid_y) and column (grid_x).
```
remove_fence(grid_y, grid_x)
```

Removes an obstacle from the grid.
```
selectObs()
```

Starts an interactive mode to place obstacles.
```
selectStart()
```
Starts an interactive mode to select the start point.
```
selectEnd()
```
Starts an interactive mode to select the endpoint.
```
hollow_obstacles(map_array)
```
Converts solid obstacles into hollow ones using binary erosion.

## Methods and resources
### nuScenes 
We use nuscenes-devkit to get map data from nuScenes datasets (mini) and generate binary image of maps.
#### Map data
To install the map expansion, please download the files from https://www.nuscenes.org/download and copy the files into your nuScenes map folder, e.g. /data/sets/nuscenes/maps.
#### Initialization
We will be working with the `singapore-onenorth` map. The `NuScenesMap` can be initialized as follows:
```
import matplotlib.pyplot as plt
import tqdm
import numpy as np

from nuscenes.map_expansion.map_api import NuScenesMap
from nuscenes.map_expansion import arcline_path_utils
from nuscenes.map_expansion.bitmap import BitMap

nusc_map = NuScenesMap(dataroot='/data/sets/nuscenes', map_name='singapore-onenorth')
```
#### Rendering binary map mask layers
The `NuScenesMap` class makes it possible to convert multiple map layers into binary mask and render on a Matplotlib figure. First let's call `get_map_mask` to look at the raw data of two layers:
```
patch_box = (300, 1700, 100, 100)
patch_angle = 0  # Default orientation where North is up
layer_names = ['drivable_area', 'walkway']
canvas_size = (1000, 1000)
map_mask = nusc_map.get_map_mask(patch_box, patch_angle, layer_names, canvas_size)
map_mask[0]
```
Now we directly visualize the map mask retrieved above using `render_map_mask`:
```
figsize = (12, 4)
fig, ax = nusc_map.render_map_mask(patch_box, patch_angle, layer_names, canvas_size, figsize=figsize, n_row=1)
```


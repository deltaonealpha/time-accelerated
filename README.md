<img src="docs/logo3.png#gh-dark-mode-only" height = "52px" width="380px" />
<img src="docs/logo1.png#gh-light-mode-only" height = "52px" width="380px" />

### Prediction library for flight dynamics using reinforced learning

Using reinforcement-learning, TimeAccelerated can predict near-future flight dynamics for aircrafts with known current parameters with a library style. Such accurate predictions and modeling can be used to adapt to known and unknown scenarios with greater efficiency.
##### Also, [Clutch, a minimalist web-viewer for TimeAccelerated data](https://github.com/deltaonealpha/time-accelerated/blob/48ec0ad91a4dcb9d74d964e7c6b51f32610d32f4/clutch.md)

TimeAccelerated uses reinforcement-learning to predict metrics, wherein an "agent" (work doer) interacts with an "environment" (work bearer). The subsequent interaction is then categorized as desirable or not, and a numeric reward counter is adjusted accordingly.
<br /><br />
<p align="center">
  <img src="docs/ta_learning_loop.png#gh-dark-mode-only" />
  <img src="docs/ta_learning_loop_light.png#gh-light-mode-only" />
</p>

## Pre-run commands
>The following examples are based on the default Python interpretation of the library.
#### Import the library
`import timeaccelerated as ta`
#### Declare parameters
```
# file imports
model = file.open("<aircraft_model>") #imports file <aircraft_model> with model information
data = file.open("<data.csv>") #imports file <data.csv> with TA required data (refer further)

# data parsing
data = ta.parse(datafile)
```
#### Initialize the library
`ta.process_initiatlize() #starts the TA daemon and initializes a RL session`

## Functions
The following are included by default:
1. `predict()` Using enhanced reinforcement-learnt models, attempts to predict near-future aircraft dynamics for a time variable in the future, <time_difference_in_seconds>, and returns an updated data object.
	
2. `simulate()` Runs a standard/ daemon process to simulate dynamics on the model declared. Multiple engine options are available, including [AirSim](https://github.com/microsoft/airsim/) and a custom development fork of it, [DeSimulate](https://github.com/deltaonealpha/time-accelerated/DeSimulate).
	
3. `train()` Optionally can be used to train a custom model for the predictor function.

#### Function - predict()
`ta.process_predict(model, data, time_difference_in_seconds, target, returntype)`

#### Function - simulate()
`ta.process_simulate(model, data, time_difference_in_seconds, target, returntype)`

#### Function - train()
`ta.process_train(model) # uses reinforcement-learning by default`

Moreover, return targets and types can be configured for each of `simulate()` and `predict()`.
**What to predict/ simulate:**
- Placement in space
	`target = "position"`
- Trajectory plots
	`target = "trajectory"`
- Thrust levels
	`target = "thrust"`
- Fuel consumption
	`target = "fuel"`
- Terrain (environment) interaction
	`target = "interaction"`

Multiple targets can be passed in the form of an array, like `target = ("position, thrust, trajectory")`. However, this might increase the potential compute complexity.
**How to return data:**
- Array (recommended for further processing)
	`returntype = "array"`
- .xls Workbook format (recommended for human interpretation)
	`returntype = "xls"`
- .csv Table format
	`returntype = "csv"`
- .txt Log format
	`returntype = "txt"`


## Configurations
A configuration file can be passed while initializing TimeAccelerated to customize deliverables, and input formats. For successive calls with varying requirements, the library must be re-initialized before each varying call.
`ta.process_initialize("config.json")`
##### config.json
```
{
	vehicle {
		curve_thrust = "",
		curve_fuel = "",
		curve_noise = "",
	},
	
	environment {
		wind_speed = "", #generalized specification
		wind_direction = "", #flow direction in 2D or 3D space, generalized specification
		temperature = "", #generalized specification 
		pressure = "", #generalized specification
		humidity = "", #generalized specification
	},
	
	simulation {
		step_size = "",
		step_amount = "",
		duration = "",
		fidelity = "", #desired approximation
	},
	
	training { #optional if training is not requied
		algorithm_name = "", #leave blank for default deepQ RL
		state_variables = (<object_list>),
		episodes = ""
		batch_size = ""
	},
}
```

Other configurable parameters outside of the scope of the configuration file include <br />
Vehicle:
- Model file
- Control surface characteristics

Environment:
- Wind speed (zonal)
- Flow direction in 2D/ 3D space (zonal)
- Terrain (zonal)
- Turbulence (zonal)
- Temperature (zonal)
- Pressure (zonal)
- Humidity (zonal)

Prediction and RL:
- Observation space
- Action space and control inputs
- Reward function customization and tuning

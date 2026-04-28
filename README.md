# autoware_sample_scenario

A sample [OpenSCENARIO](https://www.asam.net/standards/detail/openscenario/) file for use with [Autoware](https://github.com/autowarefoundation/autoware) and [scenario_simulator_v2](https://github.com/tier4/scenario_simulator_v2_scenarios).

This repository is intended as a starting point for users who want to try running Autoware in a scenario-based simulation, or who want a small reference scenario to adapt for their own testing.

## What's Inside

- [sample-scenario.yaml](sample-scenario.yaml) — a minimal OpenSCENARIO file that spawns a single ego vehicle, sends it to a goal pose, and ends the run on success (goal reached) or failure (timeout / stuck).

## Prerequisites

Before running this scenario, make sure you have:

1. **Autoware** installed and built — see the [Autoware installation guide](https://autowarefoundation.github.io/autoware-documentation/main/installation/).
2. **scenario_simulator_v2** installed alongside Autoware — see the [scenario_simulator_v2 documentation](https://tier4.github.io/scenario_simulator_v2-docs/).
3. **The sample map** downloaded from the [Autoware planning simulation tutorial](https://autowarefoundation.github.io/autoware-documentation/main/demos/planning-sim/#download-the-sample-map). After unpacking, you should have a directory containing both `lanelet2_map.osm` and `pointcloud_map.pcd`.

## Setup

The scenario file references the sample map by absolute path. You will need to update these paths to point to wherever you extracted the map on your machine.

Open [sample-scenario.yaml](sample-scenario.yaml) and update the two fields under `RoadNetwork`:

```yaml
RoadNetwork:
  LogicFile:
    filepath: /path/to/your/sample-map-planning/lanelet2_map.osm
  SceneGraphFile:
    filepath: /path/to/your/sample-map-planning/pointcloud_map.pcd
```

- `LogicFile.filepath` should point to your `lanelet2_map.osm`.
- `SceneGraphFile.filepath` should point to your `pointcloud_map.pcd`.

If these paths are wrong, the simulator will fail to load the map and the scenario will not run.

## Running the Scenario

Once the paths are updated, you can launch the scenario with scenario_simulator_v2. Refer to the [scenario_simulator_v2 quick start](https://tier4.github.io/scenario_simulator_v2-docs/user_guide/scenario_test_runner/QuickStart/) for the exact command for your setup — typically something like:

```bash
ros2 launch scenario_test_runner scenario_test_runner.launch.py \
  architecture_type:=awf/universe/20250130 \
  record:=false \
  use_custom_centerline:=true \
  scenario:=/home/aw/autoware_sample_scenario/sample-scenario.yaml \
  vehicle_model:=sample_vehicle \
  sensor_model:=sample_sensor_kit
```

## Scenario Overview

The included scenario does the following:

- Spawns one ego vehicle on the sample map.
- Issues a routing action to a goal pose further along the road.
- **Succeeds** when the ego vehicle reaches the goal and comes to a stop.
- **Fails** if the ego is stuck (standstill for 60 s) or if the simulation runs longer than 180 s.

Feel free to copy this file as a template and modify the start position, goal position, or end conditions to suit your own tests.

## Troubleshooting

- **Map fails to load** — double-check the absolute paths in `LogicFile` and `SceneGraphFile`.
- **Ego does not move** — make sure Autoware is running with the matching `architecture_type` and that the map paths in your Autoware launch arguments also point to the same sample map directory.
- **Scenario exits with failure immediately** — verify that the `laneId` values in the scenario still correspond to lanes in the map version you downloaded; if you are using a different map, these IDs will need to be updated.

## Contributing

Issues and pull requests are welcome — especially additional sample scenarios that demonstrate other Autoware features.

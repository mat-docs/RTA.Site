# Features

[Grafana](https://grafana.com/) is awesome.
It's become the mainstay for infrastructure monitoring, and we use it too.

[McLaren ATLAS](https://www.mclaren.com/applied/catalogue/item/atlas/) occupies a similar role for engineering telemetry.  
It's a sophisticated desktop client, offering live monitoring, high-resolution display and local signal processing.

Here are a few of the features:

## Workbooks

ATLAS workbooks let you setup pages of charts and read-outs:

- Build your workbooks easily with our drag-and-drop docking displays;
- Tab quickly and easily between a system overview and specialized sub-system pages;
- Share workbooks with colleagues and add your own customization.

ATLAS supports 4K screens and multiple monitors:  
Just drag displays where you need them &mdash; or clone the whole page group to keep an eye on multiple systems.

## Data Management

Organise data acquisition into telemetry _sessions_.

These logical data sets can be tagged with user-defined metadata &mdash; during or after acquisition &mdash; so you can:

* Browse and search, to go straight to specific telemetry sets
* Separate data by equipment, and trace to specific hardware and configurations
* Enrich with signal processing and simulation
* Compare data sets against models to check for divergence

!!! example "Example: Vehicle Development"
    Using a driver-in-the-loop simulator, you can capture sessions for the driver inputs, vehicle model and motion platform &mdash; and capture the precise model setup as metadata.
    You can then repeatedly re-run the virtual test route, comparing session data to see how the driver and vehicle reaction changed &mdash; for example, with firmer dampers, or a different seat position.

    This development pattern enables systematic experimentation and efficient use of simulator time.

## Live Monitoring 

ATLAS is designed from the ground-up for both live monitoring and comparative telemetry analysis.

You can jump into live session acquisition at any point, which means ATLAS is ideal for use alongside central data acquisition and dashboarding. If you see an anomaly or an out-of-tolerance parameter, you can open the session on demand.

!!! info "Use Case: Formula One"
    We use this capability to provide a service to the FIA for F1 race monitoring.

    Stewards use our dashboards to keep an overall eye on key performance metrics, and jump into live ATLAS sessions to spot-check conformance with regulations and to investigate incidents.

The live streaming protocol is designed to work securely across sites.  
Need to monitor US equipment from your Japan office? No problem.

ATLAS also has the capability to record directly from the local network.
This is ideal for benchtop equipment testing and data acquisition in the field &mdash; a capability used extensively in motorsport.
Our sales team can provide more information and a quote for custom recording.

## Compare Sets

Load up multiple acquisition sessions for comparison, and charts respond dynamically to show colour-coded overlays and legends. This capability is often used to compare multiple runs, or to contrast simulations with real-world data.

You can do this with multiple data sets at a time: flip between equipment with a single key-press.

## Intelligent Displays

ATLAS intelligently re-samples data to ensure that you never miss transients and outlying samples.
Most time-series systems will perform simple aggregations, but ATLAS is built with the assumption that transient spikes are important and need to be seen.

Panning and zooming is smooth and responsive. Zoom out to see trends, and zoom in for detail without waiting around for data to refresh from the network.

You can take ad-hoc measurements of signals using our Reference Cursor feature.
Just click and drag to measure data between points &mdash; delta, min, max, mean & standard deviation.

## Filters and Functions

Interactively cleanup data with band-pass, derivative, and integrating filters.  

Or write your own signal processing functions, in our simple expression language - or in C# or MATLAB.  
ATLAS is ideal for rapid-prototyping DSP functions for your ingest pipeline.

## Flexible X-Axis

You can drag and drop sessions to overlay them for comparison &mdash; but sometimes it doesn't make sense to plot your data against time at all. In motorsport, for example, it's very common to look at data in terms of lap distance, so that a single moment of lost time doesn't throw the rest of the data comparison out of alignment.

In ATLAS, you can choose any suitable parameter as an X-axis for your waveforms.

## Printing

Sometimes, paper is easiest.

ATLAS includes high-resolution printing support.

## Plugin Architecture

ATLAS is built around a plugin system &mdash; and you can create your own displays to suit your engineering problem, or load specialized data sets directly into the client.

Custom displays have included:

* Simulated car dashboards
* Thermal camera imagery
* ROS file replay (for autonomous vehicles)
* 2D and 3D LIDAR plots

<video width="640" controls>
  <source src="../assets/lidar.mp4" type="video/mp4">
</video>
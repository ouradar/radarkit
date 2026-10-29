# Python RadarKit Utilities

Some wrapped RadarKit functions and Python utilities for handling data


## Quick Start

The following plot can be produced as:

```python
import radarkit

rkid = radarkit.open(FILE)

out = rkid.read()

rkid.free()
```

The output is:
```
out = {
    'riq': np.array() ... # raw I/Q
    'ciq': np.array() ... # compressed I/Q
    'el': np.array() ... # elevation angles
    'az': np.array() ... # azimuth angles
}
```

## Customization

```python
import radarkit

rkid = radarkit.open(FILE)

# Configure various processing parameters
config = rkid.get_current_config()
config.SQIThreshold = 0.0
config.SNRThreshold = -3.0

rkid.set_moment_method("pp")

out = rkid.read()

prods = rkid.get_moment()

rkid.free()
```

## Retrieve Radar Products

```python
prods = rkid.get_moment()
```

## Radar Products as Plain Dictionary

```python
import radar
import radar.chart

sweep = {
    "kind": "Plain",
    "txrx": "M",
    "time": prods["time"],
    "latitude": rkid.desc.latitude,
    "longitude": rkid.desc.longitude,
    "sweepMode": "ppi",
    "sweepElevation": rkid.configs[0].sweepElevation,
    "sweepAzimuth": rkid.configs[0].sweepAzimuth,
    "prf": rkid.configs[0].prt[0] ** -1,
    "waveform": rkid.configs[0].waveform.contents.name.decode("utf"),
    "gatewidth": rkid.configs[0].pulseGateSize * rkid.desc.pulseToRayRatio,
    "elevations": prods["elevations"],
    "azimuths": prods["azimuths"],
    "ranges": prods["ranges"],
    "products": {
        "Z": prods["Z"],
        "V": prods["V"],
        "W": prods["W"],
        "D": prods["D"],
        "P": prods["P"],
        "R": prods["R"],
    },
}

chart = radar.chart.ChartSinglePPI(sweep)
```

![PPI](https://arrc.ou.edu/static/ppi-px1000.png)

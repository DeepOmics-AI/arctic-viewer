title: Ensemble
---

# Introduction

The ParaView ArcticViewer is able to load several type of datasets, but this guide will focus on the ensemble one and will explain the requirements for it so you can create your own datasets.

# Dataset structure

ParaView ArcticViewer expects a dataset descriptor that formalizes the data convention so it can be understood by the application.  The application expects a file named __index.json__ at the root of the tree structure (if any) with a content similar to the following one:

```js
{
    "type": [
        "ensemble-dataset"
    ],
    "Ensemble": {
        "datasets": [
            {
                "name": "Velocity",
                "data": "hydra-image-fluid-velocity/index.json"
            },{
                "name": "Velocity2",
                "data": "hydra-image-fluid-velocity/index.json"
            },{
                "name": "VelocityFree",
                "data": "hydra-image-fluid-velocity/index.json"
            },{
                "name": "Earth",
                "data": "mpas-probe-flat-earth/index.json"
            },{
                "name": "Earth2",
                "data": "mpas-probe-flat-earth/index.json"
            },{
                "name": "EarthFree",
                "data": "mpas-probe-flat-earth/index.json"
            }
        ],
        "binding": [
            {
                "datasets": [ "Velocity", "Velocity2" ],
                "arguments": [ "phi", "theta", "time" ]
            },{
                "datasets": [ "Earth", "Earth2" ],
                "arguments": [ "time" ],
                "other": [
                    {
                        "listener": "onProbeChange",
                        "setter": "setProbe"
                    },{
                        "listener": "onRenderMethodChange",
                        "setter": "setRenderMethod"
                    },{
                        "listener": "onCrosshairVisibilityChange",
                        "setter": "setCrossHairEnable"
                    }
                ]
            }
        ],
        "operators": [
            { "name": "Velocity diff", "datasets": [ "Velocity", "Velocity2", "VelocityFree" ], "operation": "Velocity - VelocityFree" },
            { "name": "Earth diff", "datasets": [ "Earth", "Earth2", "EarthFree" ], "operation": "Earth - EarthFree" }
        ]
    },
    "metadata": {
        "title": "Ensemble demo"
    }
}
```

In the above meta description, we find the list of datasets that compose the ensemble, along with the relationship that should exist between them.  Like synchronizing the __time__ across several dataset or any particular argument or parameter.  We can also see above and example of the kind of binding which needs to happen directly on the ImageBuilder instance.

The path provided for the datasets is relative to the initial index.json file.

Then we can add some Operator views of those dataset. These operators will allow you to apply pixel operations between the set of datasets you've listed in them.

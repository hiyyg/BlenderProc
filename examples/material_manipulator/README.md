# Material selection and manipulation

![](rendering.jpg)

In this example we demonstrate how to select materials in the scene using `getter.Material` and then manipulate them using the `MaterialManipulator` module.

## Usage

Execute this in the BlenderProc main directory:

```
python run.py examples/material_manipulation/config.yaml examples/material_manipulation/scene.obj examples/material_manipulation/output
```

* `examples/material_manipulation/config.yaml`: path to the configuration file with pipeline configuration.
* `examples/material_manipulation/scene.obj`: path to the object file with the basic scene.
* `examples/material_manipulation/output`: path to the output directory.

## Steps

* Loads `scene.obj`: `loader.ObjectLoader` module.
* Creates a point light: `lighting.LightLoader` module.
* Sets two camera positions: `camera.CameraLoader` module.
* Selects objects based on the condition: `material.MaterialManipulator` module.
* Change some parameters of the selected entities: `material.MaterilManipulator` module.
* Renders rgb: `renderer.RgbRenderer` module.
* Writes the output to .hdf5 containers: `writer.Hdf5Writer` module.

## Config file

### MaterialManipulator

```yaml
 {
  "module": "materials.MaterialManipulator",
  "config": {
    "selector": {
      "provider": "getter.Material",
      "conditions": {
        "name": "Material"
      }
    },
    color_link_to_displacement: 1.5
  }
},
```

The focus of this example is the MaterialManipulator module and `getter.Material` which allow us to select multiple materials based on a user-defined condition and change the attribute values of the selected materials.
* `selector` - section of the `MaterialManipulator` for stating the chosen `provider` and the `condition` to use for selecting.

Our condition is: `"name": 'Material'`, which means that we want to select all the materials with `material.name == 'Material'`. In our case we have only one material which meets the requirement.
Yet one may define any condition where `key` is the valid name of any attribute of entities present in the scene.
This way it is possible to select multiple materials. 

NOTE: any given attribute_value of the type string will be treated as a *REGULAR EXPRESSION*, so `"name": 'Material.*'` condition will select us all materials in the scene.

For possible `attribute_name`'s data types check `getter.Material` documentation.

After `selector` section we are defining attribute name and attribute value pairs in the familiar format of {attribute_name: attribute_value}.
If attribute_name is a valid name of any attribute of selected object(s), its value will be set to attribute_value.

With the argument: `color_link_to_displacement` will all materials, which were previously selected checked for a texture, which is then connected to the displacement output of the material.
The <float> value is used to scale the color input the texture up or down to increase or decrease the effect of the displacement.

If one wants to have values sampled once and have them set to defined attribute/properties, set `"mode": "once_for_all"` at the end of this section. 
By the default it is `"once_for_each"`.

## Visualization

Visualize the generated data:

```
python scripts/visHdf5Files.py examples/material_manipulation/output/0.hdf5
```

## More examples

* [entity_manipulation](../entity_manipulation): More on manipulators. 
* [camera_sampling](../camera_sampling): More on sampling for cameras.
* [light_sampling](../light_sampling): More on sampling for lights.
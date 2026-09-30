# Map guide

## Collections

- [Development maps](../study/maps/development/): 180 instances on 45 topologies.
- [Test maps](../study/maps/test/): 1,620 instances on 270 topologies.
- [Reference instances](../instances/): 18 instances on nine topologies.

Sizes are 50, 75 and 100; obstacle labels are `few`, `medium`, `full`; object placement is `random` or `border`. Each instance has 25 objects and four depots.

## Format

Example: [mapd_75x75_medium_random_objects25_topo3001_obj13001.json](../study/maps/test/75/medium/mapd_75x75_medium_random_objects25_topo3001_obj13001.json).

Filenames identify grid size, obstacle density, object placement, topology seed and object seed. Instances contain `metadata`, `grid`, `warehouses` and `objects`.

Coordinates in map JSONs are **[row, column]**. Rows increase downwards; the top-left cell is `[0,0]`. Illustration CSVs use **x = column, y = row**.

| Grid value | Cell kind |
|---:|---|
| 0 | Empty outside cell |
| 1 | Wall or shelf |
| 2 | Warehouse interior |
| 3 | Warehouse entrance |
| 4 | Warehouse exit |

Warehouse records identify entrances, exits and footprints. Entrance and exit direction is part of the mission specification.

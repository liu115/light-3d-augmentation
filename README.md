# Light pytorch-style 3D augmentation library

Some transform code is borrowed from [SpatioTemporalSegmentation ](https://github.com/chrischoy/SpatioTemporalSegmentation/blob/master/lib/transforms.py)
The dependancy is `numpy`.

## Example

```python
import numpy as np
import transforms as t

# Load some 3D point cloud file
coords, feats = read_ply('example.ply')
ROTATE_AXIS = 'z'

# Create multiple random augmentation transforms and combine with Compose
transform = t.Compose([
    t.RandomApply([
        t.ElasticDistortion([(0.2, 0.4), (0.8, 1.6)])
    ], 0.95),
    t.RandomApply([
        t.RandomTranslate([(-0.2, 0.2), (-0.2, 0.2), (0, 0)])
    ], 0.95),
    t.Random360Rotate(ROTATE_AXIS, around_center=True),
    t.RandomApply([
        t.RandomRotateEachAxis([(-np.pi / 64, np.pi / 64), (-np.pi / 64, np.pi / 64), (0, 0)])
    ], 0.95),
    t.RandomApply([
        t.RandomScale(0.9, 1.1)
    ], 0.95),
])

# Create dummy label
N, _ = coords.shape
labels = np.zeros((N, ), dtype=np.int32)

coords, feats, labels = transform(coords, feats, labels)
```

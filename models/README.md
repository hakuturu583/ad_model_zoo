# AD Model Zoo

This repository collection of ML models for Autonomous Driving.

## How to use?

### Dependency

- docker
- docker-compose
- nvidia container runtime

### Environment Variable

- NUSCENES_DIR : path to nuscenes dataset directory

### Setup Dataset

#### NuSCENES

Download all files from [project page](https://www.nuscenes.org/nuscenes#data-format).
Extract them and put them into one directory.

For example, my local environment is like this.

```bash
nuScenes-map-expansion-v1.3  v1.0-test_meta	    v1.0-trainval03_blobs  v1.0-trainval06_blobs  v1.0-trainval09_blobs
v1.0-mini		     v1.0-trainval01_blobs  v1.0-trainval04_blobs  v1.0-trainval07_blobs  v1.0-trainval10_blobs
v1.0-test_blobs		     v1.0-trainval02_blobs  v1.0-trainval05_blobs  v1.0-trainval08_blobs  v1.0-trainval_meta
```

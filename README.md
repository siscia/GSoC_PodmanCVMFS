# GSoC_PodmanCVMFS

CVMFS is FUSE filesystem used for software distribution mostly in HPC, it provides a read-only POSIX interface.
Podman is rootless containers library, similar to docker.

The goal of the project would be to enhance Podman to allow it to load layers directly from a read-only POSIX filesystem like CVMFS. A similar work is in progress for containerd.

From a preliminary talk we envision a plugin system for podman that will alow custom code to mount layers in a mountpoint.
It must be understood if podman is interested in this sort of plugin for the storage layer and what interface the plugin would provide.

Once this is sorted out we can start to evaluate more closely the project, and break it into scopes that make sense for a GSoC project.

To note that [there is already a request](https://github.com/containers/storage/issues/383) for this kind of work outside done outside our immediate circle.

## Useful resources

[CVMFS Homepage](http://cernvm.cern.ch/portal/filesystem)
[CVMFS Github](https://github.com/cvmfs/cvmfs)
[Podman Homepage](https://podman.io/)
[Podman Github](https://github.com/containers/libpod)
[Podman Storage](https://github.com/containers/storage)
[Podman pluging for crfs](https://github.com/giuseppe/crfs-plugin)

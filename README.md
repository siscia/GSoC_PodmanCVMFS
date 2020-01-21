# GSoC_PodmanCVMFS

CVMFS is FUSE filesystem used for software distribution mostly in HPC, it provides a read-only POSIX interface.
Podman is rootless containers library, similar to docker.

The goal of the project would be to enhance Podman to allow it to load layers directly from a read-only POSIX filesystem like CVMFS. A similar work [is in progress for containerd.](https://github.com/containerd/containerd/issues/3731)

From a preliminary talk we envision a plugin system for podman that will alow custom code to mount layers in a mountpoint.
It must be understood if podman is interested in this sort of plugin for the storage layer and what interface the plugin would provide.

Once this is sorted out we can start to evaluate more closely the project, and break it into scopes that make sense for a GSoC project.

To note that [there is already a request](https://github.com/containers/storage/issues/383) for this kind of work outside done outside our immediate circle.

## Useful resources

* [CVMFS Homepage](http://cernvm.cern.ch/portal/filesystem)
* [CVMFS Github](https://github.com/cvmfs/cvmfs)
* [Podman Homepage](https://podman.io/)
* [Podman Github](https://github.com/containers/libpod)
* [Podman Storage](https://github.com/containers/storage)
* [Podman pluging for crfs](https://github.com/giuseppe/crfs-plugin)

## Details and problems

The first thing I ended up looking at is the [`additionalimagestores`][https://github.com/containers/storage/blob/master/docs/containers-storage.conf.5.md#storage-options-table] (From the doc: Paths to additional container image stores. Usually these are read/only and stored on remote network shares.) 

This setting seems perfect for our use case, unfortunately it might not work for us.

The variable it is used here: https://github.com/containers/storage/blob/abb30b354de85f9e85bb454d374c5bfebbda9d51/drivers/overlay/overlay.go#L861 and it is just pre-pended to the path of the layer we want to mount.

Unfortunately the path of the layer comes in the format `overlay/l/XMXW6ERNXS2BPZXP23PXJBTWTV`, where `XMXW6ERNXS2BPZXP23PXJBTWTV` is a random number and the whole path is a symlink to the unpacked layer directory.

The random number is generated for each layer and written in a file, then read back.

Moreover, I was unable to trace back the name of the directories in podman from the original hash of the layers.

It is still not clear to me how those directories are created and by whom. 
I may have overlooked something here.

Definitely more investigation is needed.
